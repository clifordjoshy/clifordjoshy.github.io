---
title: "The Joy of Side Quests"
date: 2025-02-15T17:36:51+05:30
tags: ["technical", "personal", "web"]
---

While I'm absolutely someone who finds great joy in finishing every side quest in a game, this post unfortunately isn't about video games. One of the most satisfying feelings in life is when you're able to solve a problem that you have in life by yourself (and a bit of internet research, of course), be that repairing some tech, designing a CAD model that fits perfectly into some nook, or even just turning something off and on again.


## The Problem

I've been preparing for the GRE lately, and one thing that comes along with that is familiarizing yourself with a bunch of words to improve your vocabulary, which would then hopefully help you ace the verbal section. To begin with, I got a subscription to [GregMat](https://www.gregmat.com)(great app, would recommend) and started doing "Vocab Mountain". The way this course works is, you go through 34 sets of words each set having 30-ish words. And by the time you're thorough with all the sets, you should be all prepped.

Now you might be wondering what problem could I possibly have with this. Well, the answer to that is: I'm lazy. I'm someone who learns by repetition; so, what I would do here is go through all the words I'm unfamiliar with every other day, until I can recall them easily. However in GregMat, to get to all those words I'm unfamiliar with, I need to open every set and click through all the words I already know. That's way too many clicks! Imagine all the seconds I could be doom-scrolling instead.

In GregMat's defense, I did not put ANY research into checking if the app had some resource with similar functionality. Like a typical software engineer, my immediate thought was: I can make an app for this. VS Code was open before even reading what the current word was.

## Introducing, Gregory II

What happened to Gregory I? Well, I was talking to [Joel](https://www.linkedin.com/in/joelmathewc/) about this, and he mentioned he had a similar idea back when he was preparing for GRE. He'd set up a repository with the name Gregory and basic boilerplate, and then proceeded to forget about it. I suppose that was the demise of Gregory I (RIP). I can only hope Gregory II is a worthy successor.

{{<image src="/projects/gregory_ii/word.png" height="300px" caption="a snippet of Gregory II">}}

As you can see above, each word has three buttons associated with it. The "`SHOW`" button displays the definition of the word, following which I can mark it as "`GOT IT`" or "`NOPE`". The number in the top left indicates how many words I have left (decreasing number make caveman brain happy). Once I've gone through the entire word list, I now have a list of all the words that could confuse me, and I can focus my efforts on learning those words in particular.

{{<image src="/projects/gregory_ii/menu.png" height="300px" caption="left: the homepage,  right: the control panel">}}

Another benefit of having my own app is that I can combine word lists from multiple sources, and that is indeed what I ended up doing. Gregory II currently contains deduplicated words from the [GregMat](https://www.gregmat.com/recommended-resources) and [Magoosh](https://s3.amazonaws.com/magoosh.resources/magoosh-gre-1000-words_oct01.pdf) word lists. For more info about Gregory II (and descriptions of what the menu items above are), check out its [project page]({{<relref "/projects/gregory_ii">}}).

## Side Quests

Truly, there is no better incentive to do something than getting away from something else that you're being forced to do (the wonders of procrastination!). As always, the effort to impact ratio of these side-quests is questionable, but the joy in using your own "product" has got to be worth something, right?

When you're building something for just you, you don't need to worry about all those minor nits that come along with production software. Forget responsive design, browser compatibility, bundle performance, user experience and all those "fun" things (go wild with the `position: absolute;`). The only SLA you need to meet is "it should work on my device" and that feels truly liberating. Carve out a nice little subdomain, and you've got yourself an app that you can use to learn GRE words on the go and has every feature you want.