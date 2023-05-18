---
title: "Toy Compiler in Haskell"
date: 2022-03-29T17:51:02+05:30
tools: ["haskell", "alex", "happy"]
resources:
  - name: "featured-image"
    src: "cookie.jpg"
---

## Links

[:(fa-icon fa-brands fa-github):&nbsp;GitHub](https://github.com/clifordjoshy/expl-compilerlab)&emsp;
[:(fa-icon fa-solid fa-book):&nbsp;eXPL Docs](https://silcnitc.github.io/)

## About

This is a toy compiler for ExpL(Experimental Language) done as part of the Compiler Laboratory course I took in college.

The compiler was written in Haskell using the [Alex](https://www.haskell.org/alex/) lexer and [Happy](https://www.haskell.org/happy/) parser.

It compiles the source language ExpL(spec [here](https://silcnitc.github.io/expl.html) and [here](https://silcnitc.github.io/oexpl-specification.html)) to the low-level XSM (eXperimental String Machine) machine code (ABI [here](https://silcnitc.github.io/abi.html)) which runs on ExpOS (Experimental Operating System) a simulator for which can be found [here](https://silcnitc.github.io/install.html#navxsm).

## Personal Notes

This was my first delve into functional programming. It was a really different way of looking at problems and implementing a compiler in a purely functional language like Haskell was quite fun.

There were quite a few hiccups along the way for sure. I found it quite hard to wrap my head around the State Monad and struggled quite a bit in the late stage implementations of the parser. Was relatively smooth once I had that understanding though. (special mention to my college senior Farseen for the suggestion and subsequent help)

---

\* Pictured above is my cat Cookie being my rubber duck throughout the implementation of this. (also a throwback to my dual monitor setup days)
