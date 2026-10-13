---
title: "Widgeting, GObjects and D-Bus"
date: 2026-10-10T10:46:54+05:30
tags: ["linux", "rice", "technical"]
---

## preface

Since my last post on setting up a caffeine widget, I've been drawn back into customizing my rice. It's been interesting to come back to all the widgets I wrote back in college and look at it with a couple years of experience. Notably, some event-loop blocking operations and a bunch of places where I chose to go the route of polling a shell command over using event-based triggers. I understand that the gains here are marginal (especially when I have like three Electron apps running at all times), but what kind of engineer am I if I don't try to eke out every last CPU cycle from my machine.

In this pursuit, I came across [GLib](https://www.gtk.org/docs/architecture/glib), originating from the GNOME Project / GTK. While this library offer a bunch of functionality, what intrigued me most was the [GLib Object System](https://docs.gtk.org/gobject/concepts.html#background) that offers "automatic transparent API bindings to other compiled or interpreted languages". When I was initially writing these widgets, I found all this too complicated to wrap my head around, and went the easy way of using shell commands (which probably just used the same library internally!). Now that I've rewritten everything, this post is just a reason to force myself to look into how exactly the internals of this are implemented and hopefully document the details for when I revisit these widgets in a couple years.

{{<image src="https://imgs.xkcd.com/comics/tech_loops.png" height="350px" caption="relevant xkcd" href="https://xkcd.com/1579/">}}

## GObject Introspection

GObject is essentially a library that provides object oriented capablities for C, and uses those same object definition APIs along with some generic glue code that allows other languages (that can already interop with C libraries) to instantiate and use these objects. The way this works is: we define a struct type, define the methods (following some strict naming conventions) and call some macros to add a bunch of boilerplate. That gives you an object (as in OOP) in C. Once you have the source files along with the compiled shared object, you can run [`g-ir-scanner`](https://gi.readthedocs.io/en/latest/tools/g-ir-scanner.html) which "introspects" through all of them, parsing the source files and using a small C program (called the "dumper") that links against your `.so` to query type info and finally generate a `.gir` file (an XML representation of your object). [`g-ir-compiler`](https://gi.readthedocs.io/en/latest/tools/g-ir-compiler.html) then converts the `.gir` file into a `.typelib` file, which is a compact, binary version of the `.gir`, that can be directly loaded into memory.

{{< admonition tip Note >}}

You can find all the `.gir` files for the GI libs you have installed on your system at `/usr/share/gir-1.0/`

{{< /admonition >}}

For a dynamically typed language, you can import a GObject Introspectable library using the generic GI language bindings which finds the relevant `.typelib` and infers all properties and methods at runtime (there's also projects that can generate type-stubs from the `.gir` files). For statically typed languages, you'll need to use a code gen tool that reads the `.gir` files and produces concrete bindings in the target language, which you can then use in your programs.


Of course, this is ultimately all just a convention and there's probably a bunch of other ways to do this (and tools that do it), but I suppose enough people following the same convention makes it a standard. For what it's worth, I found this pretty cool. Also a disclaimer: the above is just a quick overview based on my understanding, the proper documentation is linked below.

[^glib1] [^glib2]

## D-Bus

The second (and more important to an event-based widget) bit of this journey is [D-Bus](https://www.freedesktop.org/wiki/Software/dbus/). D-Bus (short for "Desktop Bus") is essentially just a means for IPC. There's a _dbus_ daemon that any process can connect to via a socket or a TCP port. [Applications](https://www.freedesktop.org/wiki/Software/DbusProjects/) can query for the availability of objects, call remote methods on them, and request notification for the signals they emit. The daemon exposes a system-scoped  _system bus_ as well as a user-scoped _session bus_.

"Objects" in D-Bus are uniquely identified through the combination of a bus name and object path. The bus name uniquely identifies a client application connected to the bus and the object path uniquely identifies the object within that application. Each application is automatically assigned a unique and effectively random bus name upon connection. Applications can alternatively request "well known" bus names to provide a simple and consistent location for other applications to access at run time.

Every object supports particular methods and may emit particular signals which are collectively known as the object's members. All of an object's members are specified in "interfaces" (pretty much the same as their namesakes in Java).


{{< admonition tip Note >}}

There are some [standard interfaces](https://dbus.freedesktop.org/doc/dbus-specification.html#standard-interfaces) that are noted in the D-Bus that most objects implement for standard functionaly. For example, there's `org.freedesktop.DBus.Introspectable` which allows clients to see all members and properties from an object at runtime.

{{< /admonition >}}

[^dbus]

## a simple library

### the plan

With all that text out of the way, let's now try to design a simple GI library to query the `org.freedesktop.ScreenSaver` bus. This is a "well-known" bus that came up when we were working on inhibiting sleep in the last post (ref [Idle Inhibition Service Specification](https://specifications.freedesktop.org/idle-inhibit/0.1)). The specification puts forth the `org.freedesktop.ScreenSaver` interface that implements two methods.

```c
Inhibit (IN String application_name, IN String reason_for_inhibit, OUT UInt32 cookie);
UnInhibit (IN UInt32 cookie);
```

And this is exactly what we see when we "introspect" this interface on the object (this is using the `Introspectable` interface noted above). For me, this response is coming from XScreenSaver, but most DEs also follow the same spec, with only one application being able to "own" the bus.

``` bash
# busctl introspect SERVICE OBJECT [INTERFACE]

$ busctl introspect --user org.freedesktop.ScreenSaver /org/freedesktop/ScreenSaver org.freedesktop.ScreenSaver
NAME       TYPE   SIGNATURE RESULT/VALUE FLAGS
.Inhibit   method ss        u            -
.UnInhibit method u         -            -
```

Our simple library will call the above methods, and we can then use `dbus-monitor` to spy on this bus and see if our calls are getting there as intended. Our journey to conquer inhibition nears it's end!

{{<image src="https://imgs.xkcd.com/comics/command_line_fu.png" height="250px" caption="another relevant xkcd" href="https://xkcd.com/196/">}}

### the code

We'll create a library `MyLib` with the `Inhibitor` object (these names are important, and any references to them below are strictly required). We start with the header file, comments inline.

```c
#pragma once
#include <glib-object.h>

// for cpp compatibility
G_BEGIN_DECLS

// required constant
#define MYLIB_TYPE_INHIBITOR (my_lib_inhibitor_get_type())

// macro to declare a (non-inheritable) type.
// args are camelcase, snakecase, module name, obj name and parent type
G_DECLARE_FINAL_TYPE(MyLibInhibitor, my_lib_inhibitor, MYLIB, INHIBITOR, GObject)

// methods of the MyLib.Inhibitor object
MyLibInhibitor* my_lib_inhibitor_new(void);
void my_lib_inhibitor_inhibit(MyLibInhibitor* self, const gchar* application_name, const gchar* reason);
void my_lib_inhibitor_uninhibit(MyLibInhibitor* self);

G_END_DECLS
```

Then the corresponding C file that defines these methods. Since we have a GLib dependency anyway, we'll use the D-Bus helpers from Gio.

```c
#include "my-lib-inhibitor.h"

#include <gio/gio.h>

// the fields of the class
// defined here instead of the header, so that the members remain private
// and can only be accessed through getters/setters we define
struct _MyLibInhibitor {
  GObject parent_instance;
  GDBusProxy* proxy;
  guint32 cookie;
};

// convenience macro which declares the class initialization function,
// an instance initialization function and a pointer to the parent class.
// also defines the *_get_type() function we used in the header file
G_DEFINE_TYPE(MyLibInhibitor, my_lib_inhibitor, G_TYPE_OBJECT)

// define the constructor
static void my_lib_inhibitor_init(MyLibInhibitor* self) {
  self->cookie = 0;
  self->proxy = g_dbus_proxy_new_for_bus_sync(
      G_BUS_TYPE_SESSION, G_DBUS_PROXY_FLAGS_NONE, NULL,
      "org.freedesktop.ScreenSaver", "/org/freedesktop/ScreenSaver",
      "org.freedesktop.ScreenSaver", NULL, NULL);
}

// define the dispose method to decrement ref count on GDBusProxy*
// (name doesn't have to be this strictly. just convention)
static void my_lib_inhibitor_dispose(GObject* gobject) {
  MyLibInhibitor* self = MYLIB_INHIBITOR(gobject);
  g_clear_object(&self->proxy);
  G_OBJECT_CLASS(my_lib_inhibitor_parent_class)->dispose(gobject);
}

// override class vtable to attach above method to dispose hook
static void my_lib_inhibitor_class_init(MyLibInhibitorClass* klass) {
  G_OBJECT_CLASS(klass)->dispose = my_lib_inhibitor_dispose;
}

// ------ METHOD DEFINITIONS  ------

// the annotations on the functions below are important.
// it indicates memory ownership, parameter flow and nullability

/**
 * my_lib_inhibitor_new:
 *
 * Creates a new #MyLibInhibitor instance.
 *
 * Returns: (transfer full): A newly created #MyLibInhibitor.
 */
MyLibInhibitor* my_lib_inhibitor_new(void) {
  // not strictly required. just a convention since this method has stricter types than the void* that g_object_new returns

  return g_object_new(MYLIB_TYPE_INHIBITOR, NULL);
}

/**
 * my_lib_inhibitor_inhibit:
 * @self: A #MyLibInhibitor instance.
 * @application_name: The name of the application requesting the inhibition.
 * @reason: The reason for the inhibition.
 *
 * Requests the screen saver inhibition.
 */
void my_lib_inhibitor_inhibit(MyLibInhibitor* self, const gchar* application_name, const gchar* reason) {
  if (!self->proxy || self->cookie != 0) return;

  GVariant* res = g_dbus_proxy_call_sync(
      self->proxy, "Inhibit", g_variant_new("(ss)", application_name, reason),
      G_DBUS_CALL_FLAGS_NONE, -1, NULL, NULL);

  if (res) {
    g_variant_get(res, "(u)", &self->cookie);
    g_variant_unref(res);
  }
}

/**
 * my_lib_inhibitor_uninhibit:
 * @self: A #MyLibInhibitor instance.
 *
 * Releases a previously held screen saver inhibition.
 */
void my_lib_inhibitor_uninhibit(MyLibInhibitor* self) {
  if (!self->proxy || self->cookie == 0) return;

  GVariant* res = g_dbus_proxy_call_sync(
      self->proxy, "UnInhibit", g_variant_new("(u)", self->cookie),
      G_DBUS_CALL_FLAGS_NONE, -1, NULL, NULL);

  if (res) g_variant_unref(res);
  self->cookie = 0;
}
```

Now, a Makefile to compile this and create our `.so`, `.gir` and `.typelib` files.

```Makefile
CC = gcc
CFLAGS = -Wall -Wextra -fPIC $(shell pkg-config --cflags gobject-2.0 gio-2.0)
LIBS = $(shell pkg-config --libs gobject-2.0 gio-2.0)

LIB_NAME = inhibitor
SHARED_LIB = lib$(LIB_NAME).so
SRC_C = my-lib-inhibitor.c
SRC_H = my-lib-inhibitor.h

NAMESPACE = MyLib
VERSION = 1.0
GIR_FILE = $(NAMESPACE)-$(VERSION).gir
TYPELIB_FILE = $(NAMESPACE)-$(VERSION).typelib

all: $(SHARED_LIB) $(TYPELIB_FILE)

$(SHARED_LIB): $(SRC_C) $(SRC_H)
	$(CC) -shared $(CFLAGS) $(SRC_C) -o $(SHARED_LIB) $(LIBS)

$(GIR_FILE): $(SRC_C) $(SRC_H) $(SHARED_LIB)
	g-ir-scanner --namespace=$(NAMESPACE) \
	             --nsversion=$(VERSION) \
	             --include=GObject-2.0 \
	             --include=Gio-2.0 \
	             --library=$(LIB_NAME) \
	             --library-path=. \
	             --warn-all \
	             --output=$(GIR_FILE) \
	             $(SRC_H) $(SRC_C)

$(TYPELIB_FILE): $(GIR_FILE)
	g-ir-compiler $(GIR_FILE) -o $(TYPELIB_FILE)
```


And we're done! Let's call `MyLib.Inhibitor` from Python now.

```python
import time
import gi

gi.require_version('MyLib', '1.0')
from gi.repository import MyLib

inhibitor = MyLib.Inhibitor.new()

inhibitor.inhibit("MyPythonApp", "Running a vital long-lived migration task")

time.sleep(2)

inhibitor.uninhibit()
```

Voila!

```bash
$ dbus-monitor "type='method_call',interface='org.freedesktop.ScreenSaver'"

method call time=1791914757.940315 sender=:1.1229 -> destination=:1.9 serial=9 path=/org/freedesktop/ScreenSaver; interface=org.freedesktop.ScreenSaver; member=Inhibit
   string "MyPythonApp"
   string "Running a vital long-lived migration task"
method call time=1791914759.941449 sender=:1.1229 -> destination=:1.9 serial=10 path=/org/freedesktop/ScreenSaver; interface=org.freedesktop.ScreenSaver; member=UnInhibit
   uint32 3431596283
```

## widgeting

Now to get back to where this all started. My widget bar

{{<image src="../gnulinux/ricebar.png" width="100%" caption="picture from an old post, but nothing has changed visually">}}

You can find all of this on my [dotfiles repo](https://github.com/clifordjoshy/dotfiles/tree/master/.config/awesome), but here are the highlights of what changed. At a high-level, all the polling bits have been replaced by libraries that make use of the above two concepts to allow native event-driven widgets.

### NetworkManager

This earlier used to poll the below commands
```bash
# conn type
nmcli -f TYPE,DEVICE,STATE con show --active | awk '$3 == "activated" {printf "%s:%s",$1,$2; exit}'
# strength (if wifi)
nmcli -t -f IN-USE,SIGNAL device wifi | grep '*'
# connectivity
nmcli network connectivity
# down speed
ifstat <interface> | awk 'NR==4 {printf $6}'
```

All the `nmcli` commands have now been replaced with [libnm](https://networkmanager.dev/docs/libnm/latest/), some snippets below.

```lua
local lgi = require("lgi") -- lua lib for G-I
local NM = lgi.NM
local  nm_client = NM.Client.new(nil)

nm_client.on_notify = function(obj, pspec)
  if pspec.name == "connectivity" or pspec.name == "primary-connection" then
    local active_conn = nm_client:get_primary_connection()
    local conn_type = active_conn:get_connection_type()
    local strength = active_conn:get_devices()[1]:get_active_access_point():get_strength()
    local connectivity = nm_client:get_connectivity()
end
```

The calls to `ifstat` have now been replaced by reads to `/sys/class/net/%s/statistics/rx_bytes` which should be much faster than spawning a shell.

### UPower

The battery widget has pretty much always been using GObject, thanks to me starting from [this widget](https://github.com/berlam/awesome-upower-battery) as reference. This is where I got started on this whole refactor. The widget relies on the [UPower daemon](https://upower.freedesktop.org/) that subscribes to various udev events and makes battery information available on the `org.freedesktop.UPower` service. We can then use [libupower-glib](https://upower.freedesktop.org/docs/libupower-glib.html) to get these events into our widget.

```lua
local upower = require("lgi").require("UPowerGlib")
local upower_device = upower.Client():get_display_device()

upower_device.on_notify = function(device, pspec)
  local is_charging = upower_device.state == upower.DeviceState.PENDING_CHARGE or
    upower_device.state == upower.DeviceState.FULLY_CHARGED or
    upower_device.state == upower.DeviceState.CHARGING

  local charge = upower_device.percentage
end

```

### playerctl

This earlier used to poll

```bash
playerctl metadata -af '{{playerName}}/#/{{title}}/#/{{artist}}/#/{{status}}'
```

which was inefficient to begin with since the `playerctl` CLI has the `--follow` flag that notifies when these fields change on a particular player anyway. Regardless, the shell calls have been done away with entirely in favour of the `Playerctl` introspectable [library](https://github.com/altdesktop/playerctl/blob/master/README.md#using-the-library).

```lua
local Playerctl = require("lgi").require('Playerctl', '2.0')
playerctl_manager = Playerctl.PlayerManager()

local function on_player_event(player)
  local player_name = player.player_instance
  local title = player:get_title() or ""
  local artist = player:get_artist() or ""
  local playback_status = player.playback_status
end

local function track_player(name_obj)
  local p = Playerctl.Player.new_from_name(name_obj)

  p.on_metadata = function() on_player_event(p) end
  p.on_playback_status = function() on_player_event(p) end

  playerctl_manager:manage_player(p)
end

playerctl_manager.on_name_appeared = function(_, name) track_player(name) end
playerctl_manager.on_player_vanished = function(_, p) remove_player(p) end
```

### PulseAudio

Technically, I use `pipewire`, but `pipewire-pulse` implements all the same interfaces. Earlier, this widget used to poll

```bash
pactl get-sink-volume @DEFAULT_SINK@
```

which just like the above, was completely unnecessary, since there's `pactl subscribe` that notifies you (via stdout) for all events that happen, which is just what I did.

There does exist a D-Bus [interface](https://www.freedesktop.org/wiki/Software/PulseAudio/Documentation/Developer/Clients/DBus/) (`org.PulseAudio.Core1.Device`) that we can use to subscribe to and control the Pulse server, though it's not recommended to use the interface. However, as noted in the docs, the interface has stayed very stable, like dead things tend to do.


### DDC/CI

This widget doesn't really fit in with the rest of this post, since it doesn't use D-Bus or Glib and still relies on polling. However, since I'm on the topic of widgets, I think it's pretty cool that you can control the brightness of external monitors over HDMI and it's a shame that it's not more common. `DDC`(or [Display Data Channel](https://en.wikipedia.org/wiki/Display_Data_Channel)) is what your monitor uses to communicate information about itself to your graphics card (over I²C). `DDC/CI` (command interface) specifies a means for a computer to send commands to the monitor. If your monitor does support this, then you can use `ddcutil` to get/set the brightness. There's also the [ddcci-backlight](https://gitlab.com/ddcci-driver-linux/ddcci-driver-linux) kernel module that you can use to create a symlink to `/sys/class/backlight/` which makes it appear as a built-in monitor.

## fin

If the word counts are to believed, this is my longest blog-post yet (though I'm pretty sure the code segments here are inflating that count). Reading this again, I'm not sure how readable (or entertaining) this actually is, but I'm glad to have attempted something akin to a deep-dive on this blog. I appreciate you getting this far. This post concludes the two-part ricing sidequest arc (for now).

[^glib1]: [GObject concepts - GTK docs](https://docs.gtk.org/gobject/concepts.html)
[^glib2]: [GObject tutorial - GTK docs](https://docs.gtk.org/gobject/tutorial.html)
[^dbus]: [Introduction To DBus - Freedesktop docs](https://www.freedesktop.org/wiki/IntroductionToDBus/)