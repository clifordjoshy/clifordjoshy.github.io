---
title: "Xv6, a simple Unix-like teaching operating system"
subtitle: "a minimal Unix-like kernel for RISC-V"
date: 2024-12-03T11:31:22+05:30
tools: ["c", "qemu", "riscv"]
featuredImage: "run.png"
---

## Links

{{<iconlink "github" "GitHub" "https://github.com/clifordjoshy/xv6-implementation">}}&emsp;
{{<iconlink "reference" "Reference" "https://pdos.csail.mit.edu/6.1810/2023/xv6.html">}}

## About

From the README,
> xv6 is a re-implementation of Dennis Ritchie's and Ken Thompson's Unix
Version 6 (v6).  xv6 loosely follows the structure and style of v6,
but is implemented for a modern RISC-V multiprocessor using ANSI C.

This is an undergraduate course at MIT that has students implement a minimal Unix-like kernel with the following assignments (ref the [course schedule](https://pdos.csail.mit.edu/6.1810/2023/schedule.html))
1. util: Unix utilities
2. syscall: System calls
3. pgtbl: Page tables
4. traps: Traps
5. cow: Copy-on-write fork
6. thread: Multithreading
7. net: Network driver
8. lock: Parallelism/locking
9. fs: File system
10. mmap: Mmap

## Personal Notes

This was a meaningful continuation of the [eXpOS](https://exposnitc.github.io/) project that I implemented in college. That had a lot of the core concepts as the above, but the implementation was for a simulated multi-processor architecture (XSM) that abstracts a lot of the complexities of real-world machines. It was fun to work on something that could genuinely run on a real-world machine. Over the course of implementing this, I had to refer the official RISC-V ISA [manual](https://github.com/riscv/riscv-isa-manual) to understand some implementation details, which was also pretty cool.

I paused my implementation somewhere in the middle of Assignment 7 above, mostly because some personal stuff came up and I got lost reading the [E1000 Software Developer's Manual](https://pdos.csail.mit.edu/6.1810/2025/readings/8254x_GBe_SDM.pdf). This is definitely something I plan to pick up and finish again, possibly once I get my hands on the [Framework RISC-V Mainboard](https://frame.work/products/deep-computing-risc-v-mainboard) and can boot into my own OS!