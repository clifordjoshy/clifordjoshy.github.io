---
title: "NITCbase - a toy RDBMS"
subtitle: "a self-paced database implementation course"
date: 2023-05-07T15:21:50+05:30
tools: ["cpp", "docusaurus", "github", "markdown"]
featuredImage: "https://github.com/NITCbase/nitcbase.github.io/raw/main/static/img/FrontBannerLogo.png"
---

## Links

{{<iconlink "github" "GitHub" "https://github.com/NITCbase">}}&emsp;
{{<iconlink "website" "Website" "https://nitcbase.github.io">}}

## About

NITCbase is a course aimed at an undergraduate student that guides them through the implementation of an elementary RDBMS. The database is designed to have 8 layers covered top-down as follows:

1. **Frontend Interface**: This layer interacts with the user, and translates high level SQL-like queries from the user into calls to appropriate methods of the lower layers.
2. **Schema Layer**: All DDL (Data Definition Language) commands of NITCbase - such as commands for creating, deleting, opening, closing and renaming relations; commands for renaming attributes of a relation and so on - are directed to this layer.
3. **Algebra Layer**: All DML (Data Manipulation Language) commands of NITCbase - insert, select, project and join - are directed to this layer.
4. **Block Access Layer**: This layer implements a lot of the common logic used for the implementation of various functionality in the higher layers. It handles core functionality like searching through a disk block, inserting values into a disk block and so on.
5. **B+ Tree Layer**: This layer handles relation indexing operations using B+ trees. The implementation of all B+ tree operations are handled in this layer and an abstraction is made available to higher layers.
6. **Cache Layer**: This layer implements the run time memory data structures associated with _open_ relations of the database.
7. **Buffer Layer**: This layer is used for buffered access to the disk. All higher layers access the disk through this layer. An LRU buffer of disk blocks is maintained in memory to optimise the disk access operations in the database.
8. **Physical Layer**: This layer serves as an abstraction to handle low-level disk operations and makes available methods to read and write into disk blocks.

The project features a [roadmap](https://nitcbase.github.io/docs/Roadmap) which divides the implementation into logical stages and explains the necessary theory required for the implementation at each stage. Further details about the design can be found [here](https://nitcbase.github.io/docs/Design/Architecture).

The development of this project was carried out by a [team](https://nitcbase.github.io/AboutUs#authors) of students and faculty at my college, NITC, over a period of 4 years. I joined the team in 2022 as part of my final year project and was largely involved in the design of the roadmap, the B+ Tree Layer and ironing out the design before the project's release.

## Personal Notes

I spent almost all of my last semester in college grinding away at this project. It was tons of fun sitting in the SSL reviewing/writing the documentation with Murali Sir. I've had a lot of admiration for the [expOS](https://exposnitc.github.io) and [exPL](https://silcnitc.github.io) projects since I attempted the respective labs. It feels really nice to have been involved in the creation of the third lab in the series, the all new DBMS Lab.

Being on the side of the course designer rather than the course taker was a really interesting experience. I think I have a fundamental understanding of the NITCbase system (and database design in general, I hope!) that cannot be attained from purely attempting the project through the laid-out path. Hopefully, by the time I come back for my college reunion, this will be a well-established course. Fingers crossed!
