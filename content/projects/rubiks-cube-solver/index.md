---
title: "Rubiks Cube Solver"
date: 2021-10-01T17:50:33+05:30
tools: ["cpp", "mingw"]
resources:
  - name: "featured-image"
    src: "moves.png"
---

## Links
[:(fa-icon fa-brands fa-github): GitHub](https://github.com/clifordjoshy/thistlethwaite-rubiks)&emsp;
[:(fa-icon fa-solid fa-book): Thisthlethwaite Algorithm Reference](https://www.jaapsch.net/puzzles/thistle.htm)

## About

My implementation of a Rubik's cube solver using the Thisthlethwaite algorithm. A solution can be found for any shuffled state in 45 moves or less.

The algorithm works by splitting the solving into various stages such that
- each stage only uses a subset of the possible moves
- moves used in a stage will not be needed in any subsequent stages

With the above restrictions, an [iterative deepening depth-first search](https://en.wikipedia.org/wiki/Iterative_deepening_depth-first_search) is conducted on the state space until a solution is obtained.

The program was able to find solutions for most states in under a minute. The running speed can be further optimised by making use of look-up tables.

## Personal Notes

This was a pretty hard project to implement. The only proper piece of documentatio.n I could find for this algorithm was a letter from Thisthlethwaite linked above. I had to pour over those papers for days on end to properly understand the algorithm.