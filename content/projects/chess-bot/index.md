---
title: "Minimax Chess Bot and GUI"
date: 2020-12-09T17:40:49+05:30
tools: ["python", "cpp", "g++", "mingw", "pygame", "win32"]
resources:
  - name: "featured-image"
    src: "banner.png"
---

## Links

[:(fa-icon fa-brands fa-github):&nbsp;GitHub - Python Version](https://github.com/clifordjoshy/minimax-chess-py)&emsp;
[:(fa-icon fa-brands fa-github):&nbsp;GitHub - C++ Version](https://github.com/clifordjoshy/minimax-chess-cpp)

## About

This is my implementation of a single-player chess game using the minimax algorithm (which is an algorithm that can be used to generate optimal solutions for a zero-sum two player game) with alpha-beta pruning. A GUI was also written to make use of the bot and provide a single-player chess game experience.

There were two separate implementations that I did.

### Python Version

- This was the one I completed first and represents all the data in an object-oriented fashion.
- Contains a cross-platform GUI built using [pygame](https://www.pygame.org/wiki/about).

### C++ Version

- A complete re-implementation to have faster running time than the preceding version. Written in a procedural fashion.
- Written in GCC (ported to Windows as [MinGW](https://sourceforge.net/projects/mingw/))
- The UI was built using the [native Win32 API](https://learn.microsoft.com/en-us/windows/win32/desktop-programming) and as such, is not cross-platform. The algorithmic part was written independently of the UI leaving open the possibility of other GUIs.

## Personal Notes

This was one of my cooler projects and one I'm quite proud of to date. It does seem a lot more trivial now than it did then to make this, but I suppose that's a sign of growth.

I can't count the number of people I've recommended this project to as something to get their foot into writing some actual code. And one video that I send to them all is this explanation of the [minimax algorithm by Sebastian Lague](https://www.youtube.com/watch?v=l-hh51ncgDI). The channel is one I'm subscribed to till date and one that I'd definitely recommend you check out too.

---

{{<raw>}}
<sub>
<b>DISCLAIMER</b>: This is a project from my pre-GNU/Linux (<i>you happy, Stallman?</i>) days. Please do not crucify me for my choice of compiler and GUI. I can solemnly pledge the screenshot in the banner was from the Python version and I didn't go anywhere near Windows to run the other one.
</sub>
{{</raw>}}
