---
title: "Minesweeper, but hold the guessing"
date: 2026-11-22T15:43:51+05:30
tags: ["technical", "native-app", "web"]
---

## Preface

I used to love playing Minesweeper back in the WinXP (and later Win7) days. However, I had one big gripe with it: you spend 10 minutes solving a board only to end up in a situation where you have to make a random guess on what cell is the mine. Tried so hard, got so far, only to lose it all to that frowny face🙁. I had no choice back then, but now that I'm old enough to fight my own battles, I've finally decided to take this off my to-do list.

{{< admonition tip "In case you've forgotten what Minesweeper is" >}}

You're given a grid of hidden cells, some of which contain mines. The aim is to reveal all the safe cells without clicking on a mine. Numbers indicate how many mines are in the eight surrounding cells. Use them to identify and flag the mines, and "sweep" the grid.

{{< /admonition >}}

{{<image src="https://minesweepergame.com/image/strategy/windows-xp-minesweeper.png" height="300px" href="https://minesweepergame.com/strategy/how-to-play-minesweeper.php" caption="the WinXP version I used to play">}}

## Prior Art


Others have tried (and succeeded) in solving this annoyance. Some of the interesting implementations I've seen:
1. **[Kaboom](https://pwmarcz.pl/blog/kaboom/) by Paweł Marczewski**: This is a variant where the computer is actively playing against you. This means that if you click on a cell without making a logical deduction, it is guaranteed to be a mine. Conversely, if a group of cells cannot be opened without guessing, then any random guess amongst them is guaranteed to be safe. Definitely an interesting approach (and one I found to be too cruel 😞), and the linked blog post is an interesting read. His solver reduces the Minesweeper constraints into SAT and uses [MiniSat](http://minisat.se/) to solve it.
2. **[Minesweepr](https://mrgris.com/projects/minesweepr/) by Drew Roos**: Don't think I'm good enough at math to describe it any better than the original author. _It can solve any board, with any topology, and exactly compute every cell's chance of being a mine. It achieves this through advanced combinatorial and probability analysis._ Though not exactly a "solvable Minesweeper", I still thought this was a pretty cool way to get around random guesses (and I suppose what more [advanced players](https://minesweepergame.com/strategy/guessing.php) would intuitively do?)
3. **[Mines](https://www.chiark.greenend.org.uk/~sgtatham/quasiblog/mines-solver/) by Simon Tatham**: I'll admit that this was the inspiration for this whole project, and probably the first time I realized it was even possible to do something about this. The linked blog post along with [this one](https://www.chiark.greenend.org.uk/~sgtatham/puzzles/devel/writing.html) served as a good starting point. I think a lot of the ideas he suggests are very vibes-based, but the experience (see: his [puzzle collection](https://www.chiark.greenend.org.uk/~sgtatham/puzzles/)) really shows when those intuitions turn out to be right. I'd highly recommend reading his original post if you're setting out on a similar journey. The rest of this post covers my own attempt at these ideas, but of course, the implementation is completely independent (and in Rust!).

## Implementation

At a high level, we achieve a solvable board by running a solver that applies the same deductions a human player would. Whenever the solver gets stuck, we run a "perturber" that moves one or more mines around, allowing the solver to make at least one new deduction. We repeat this process until the board is guaranteed to be logically solvable.

### Solver

You might have already guessed from the SAT-reduction above, but solving a Minesweeper board is NP-complete ([Kaye, 2000](https://link.springer.com/article/10.1007/BF03025367)). However, for the standard board sizes (and quite a bit more), we can run the perturb-solve loop fast enough (<1s) that it's basically unnoticeable. We can of course only do this after the first click, since the start position decides the state of the board.

As I see it, the solver essentially comes down to three cases.

#### Cases
##### Count Match

```text
1.┌─────────┐  2.┌─────────┐
  |  F  [■] |    |  F  [■] |
  |  1      |    |  2      |
  └─────────┘    └─────────┘
```

This is the simplest case.
1. If the number of hidden cells around a revealed cell matches the number of the revealed cell, then every other hidden cell around it must be safe.
2. If the total number of hidden cells around a revealed cell matches the number of the revealed cell, then all of the hidden cells must be mines and can be flagged.

##### Neighbour Constraining

```text
1.┌─────────────┐  2.┌─────────────────┐  3.┌─────────────┐
  | [■]  ■   ■  |    |  F   ■   ■  [■] |    |  1   ■      |
  |      2   1  |    |      2   1  [■] |    |      ■   2  |
  └─────────────┘    |             [■] |    |     [■]     |
                     └─────────────────┘    └─────────────┘
```

The constraints imposed by one revealed cell can be narrowed down using those imposed by a neighbouring revealed cell. This allows us to flag or open cells that aren't affected by the constraining neighbour. For example,
1. In case 1 above, the highlighted cell is definitely a mine since we know for sure that only one of the other two hidden cells can be a mine (because of the 1 below them).
2. In case 2, the highlighted cells are definitely safe since we know for sure that one among the non-highlighted hidden cells must be a mine (from the 2) and therefore the 1 guarantees that the other three hidden cells will not be a mine.

A particular revealed cell can be directly constrained by revealed neighbours up to a depth of 2 (see case 3 above). In implementation, this is essentially just set intersection and complement operations across the set of hidden cells constrained by each revealed cell in the depth-2 neighbourhood.

##### Frontier Backtracking

The previous case only considers neighbouring revealed cells whose constraints overlap in the hidden cells they affect. However, it's possible that there's a chain of constraints that would allow us to make a new deduction.

```text
■  [■]  ■   ■   ■   1
1   1   1   2   ■   1
```

In the example above, the highlighted cell is definitely not a mine. We know that since only one amongst the rightmost hidden cells can be a mine, which implies that only one among the hidden cells at column 3 and 4 can be a mine. Therefore, from the 1 at column 3, we can deduce that the highlighted cell is safe.

Such chains can extend to any length, so we'll rely on backtracking through the unknown region to find more deductions. Before that, some terminology:
1. **Frontier**: All the hidden cells that are directly being affected by one or more revealed cells. Marks the edge of the unknown region.
2. **Interior**: Every hidden cell that's not part of the frontier.

Essentially, what we'll do is backtrack across all mine positions in the frontier and shortlist those that satisfy all the known constraints. Any cell that was flagged across every valid solution is guaranteed to be a mine and any cell that isn't flagged in any valid solution is definitely safe.

This is the single most time-consuming step of the whole process, and likely the one with the most scope for future improvements. In any case, here are some easy optimizations to reduce the depth of the search space.
1. The frontier can be split into disjoint components, based on the overlap of revealed cells that constrain each component. Think of a board with the centre entirely revealed, with two pools of hidden cells on either end. There's nothing connecting both these frontiers, so that breaks it down to two separate searches instead of one big tree. And in the world of exponents, that's a significant saving!
2. Immediately prune any branch where some constraint is violated (i.e. the mine count exceeds the number).
3. We can easily estimate the minimum and maximum mines that a possible solution in each frontier would require. Prune any branches that fall outside these limits.
4. Towards the end of the game, we can use the number of mines remaining to be marked as an additional indicator to select solutions and make deductions about the interior. For example, if every valid frontier configuration uses 2 mines, and there's only 2 mines left to be marked, then we can deduce that the entire interior must be safe (+ a similar deduction for the entire interios being mines).
5. (definitely more stuff to be figured out)


#### Loop

1. Run the two simple cases across the board.
2. If the simple cases run out of moves, run the expensive case until we make progress on each disjoint component of the frontier. Then try the simple cases again.
3. When even backtracking is unable to proceed, we've exhausted all possible logical deductions!!

{{<image src="./stuck.png" height="300px" caption="4 mines to flag. No logical deductions left to make.">}}

### Perturber

This step is surprisingly simple, and very much inspired by the aforementioned post. The idea is that we'll make the simplest set of changes possible to unblock at least one new deduction.

1. Find the smallest unknown region in the frontier that is constrained by a revealed cell.
2. Randomly decide whether to transform the entire region into mines or safe cells.
3. Based on that decision, look for mine donors or receivers in the interior. If we're unable to find enough donors or receivers in the interior (perhaps towards the end of the game, when the interior is likely to be small), we fall back to the frontier. If that still isn't enough, we start looking in the already revealed region. We prefer changing things around in the interior and frontier since that's much less likely to affect any of the deductions we've already made to get this far.
4. Once we have identified the pairs, make the swaps and re-hide the region around the newly transformed cells.
5. This counts as one "perturbation".

Then we re-run the solver and hope we make more progress. If the solver does run all the way through, great! But we're not done just yet. In moving the mines around, we might have messed up one of the deductions we made to get to that point. To confirm, we reset the state of the board and re-run the solver. If it's still able to solve the board, we're done and ready to begin the game.

If not, then we need to re-run the perturber. But how many times will we keep doing that? Well, a simple (vibes-based) heuristic for that is, each subsequent run of the perturber must take fewer perturbations to get to a solved state. If it does take more, then we've made the board worse with this round of perturbations. Time to abandon ship and start over with a whole new board. Based on some rather non-rigorous local testing, a 30×24 board with 200 mines usually only takes up to 3 rounds of perturbations before it becomes solvable. In any case, all the optimizations from earlier were just to ensure that we're able to run through all of this before the user notices any delay. Across all standard difficulty levels, we're able to generate a solvable board in less than 100 ms. Good enough, I say!

There are some edge cases I've not mentioned above, for example, when the interior is completely enclosed by flagged cells, leaving us with no frontier and no deductions to proceed with. If you've made it this far, I trust you to figure this one out.

## Guesses be gone

And we're done! We have a logically solvable minesweeper game at our fingertips. Onwards to the UI.

This was my first experience with Rust, and I've got to say, I can see the appeal. All the syntactic sugar and the general functional programming paradigms when working with iterators are quite nice! As with all beginners, I started off fighting the borrow checker, but it eventually started making perfect sense. I used [egui](https://github.com/emilk/egui) to build the app, mostly because it seemed like the least effort to build a simple GUI. Overall, the Rust ecosystem genuinely seems pretty solid, and all the [No Boilerplate](https://www.youtube.com/playlist?list=PLZaoyhMXgBzoM9bfb5pyUOT3zjnaDdSEP) Rust videos filling my YouTube feed definitely helped push me along. I'm glad to finally feel deserving of the Rust sticker I've been carrying around on my laptop for the past couple of years. Kerala Rustaceans, here I come!

Another benefit of building it in Rust is that it's pretty straightforward to compile it into WASM. The WebAssembly build of `minesweep.rs` is available at [/demos/minesweeper](/demos/minesweeper), and in the iframe below. Hope I've drilled enough interest in you to try your hand at one game at the very least. Good luck!

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