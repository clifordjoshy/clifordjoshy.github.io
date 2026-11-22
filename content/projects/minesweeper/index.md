---
title: "Minesweep.rs - an oxidized no guess Minesweeper"
subtitle: "an implementation of Minesweeper that guarantees that boards are solvable without having to guess"
date: 2026-11-21T09:47:50+05:30
tools: ["rust", "egui", "wasm"]
---

{{<image src="/demos/minesweeper/icon.svg" height="40px" alt="icon">}}

## Links

{{<iconlink "github" "GitHub" "https://github.com/clifordjoshy/minesweep.rs">}}&emsp;
{{<iconlink "website" "Demo" "/demos/minesweeper">}}&emsp;
{{<iconlink "reference" "Simon Tatham's Blog" "https://www.chiark.greenend.org.uk/~sgtatham/quasiblog/mines-solver/">}}

## About

The original version of Minesweeper in Windows XP does not guarantee that every board is solvable, meaning you can end up in a situation where you have to make a random guess to proceed. This implementation aims to generate boards that can be solved using logical deductions alone, inspired by Simon Tatham's implementation of the same idea. While I referred to his excellent [blog](https://www.chiark.greenend.org.uk/~sgtatham/quasiblog/mines-solver/) for guidance, the implementation is my own.

We achieve this by running a solver that applies the same deductions a human player would. Whenever the solver gets stuck, we run a "perturber" that moves one or more mines around, allowing the solver to make at least one new deduction. We repeat this process until the board is guaranteed to be solvable. I've gone into more detail about the approach in the accompanying [post]({{<relref "/posts/minesweeper">}}).

Solving a Minesweeper board is NP-complete ([Kaye, 2000](https://link.springer.com/article/10.1007/BF03025367)). However, for the standard board sizes (and quite a bit more), we can run the perturb-solve loop fast enough (<1s) that it's basically unnoticeable. We can of course only do this after the first click, since the start position decides the state of the board.

Some quick benchmarks on time taken to build a solvable board, sampled across a 1000 random boards.

| Difficulty | Mine Density | Median | P99 |
| --- | --- | --- | --- |
| Beginner (10×10, 10)| 10% | 0.05ms | 0.16ms|
| Intermediate (16×16, 40) | 15.6% | 0.21ms | 0.77ms |
| Expert (30×16, 99) | 20.6% | 1.24ms | 4.08ms |
| Master (30×20, 200) | 33.3% | 6.04ms | 24.7ms |
| _benchmark only_ (30×20, 400)| 66.6% | 41.5ms | 175ms|


## Personal Notes

This was my first time actually using Rust to build something, and I can definitely see the appeal. It feels like a nice modern language with all the syntactic sugar and is fast too. On the project, this has been on my to-do list for the longest time, and I'm glad I finally have my very own solvable Minesweeper!

Another benefit of building it in Rust is that it compiled to WASM with little to no effort at my end. And thanks to that, I can offer to you a live working demo right here on this page.

## Demo

The WebAssembly build of `minesweep.rs` is available at [/demos/minesweeper](/demos/minesweeper), and in the iframe below.

{{<raw >}}
<iframe
 src="/demos/minesweeper"
 style="width:100%;
        aspect-ratio:3/2;
        border-radius:12px;
        min-height: 500px;
        margin-top: 20px;
        "
  loading="lazy"
  title="minesweep.rs"
></iframe>
{{</raw >}}