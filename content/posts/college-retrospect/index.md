---
title: "College: Calicut, Coding and COVID"
date: 2023-05-17T16:28:59+05:30
tags: ["college", "retrospect", "personal"]
---

A week back, my undergrad college ([NITC](https://nitc.ac.in)) education came to an end. Though I spent around a half of it sitting at my desk at home staring at a screen, I'm still leaving the place with a bunch of fond memories, some great experiences and an itch to talk about all of it. Luckily, I have this blog which has patiently been waiting for me to come scream into the void again.

This article can largely be likened to a journal entry than a blog post, but still, I think there's a lot of fun stuff in NITC that's worth talking about. This article will hopefully also serve as a neat little reminder when I come back to this page next year. In no particular order, here are some snippets from my _college memories_.

## Stickers

Before I bore you with my nostalgic infodumping, I think I need to grab your attention with some colours. Behold, my laptop!

{{<image src="stickers.jpeg" height="300px">}}

Collecting these stickers was like a side-quest for me throughout college. Over the span of my final year in college, I managed to attend and participate in enough events to fill up my laptop with stickers, each of which bring back unique memories. I don't know if this laptop will make it another 10 years (silent prayer to the ThinkPad gods🙏), but here lies the proof that I finished my quest!

## SSL

The SSL, or the Software Systems Lab is a room belonging to the CSE department of NITC with around 80 Debian PCs. By day, this lab functions as a venue to conduct lab courses and such. By night, the _cold_ air settles into the soul of the people striking into the depths of their hearts a feeling of ✨the vibes✨.

### The Room

In all seriousness though, the SSL is something I find really cool at college. A group of students are given the keys to the lab and dubbed `SSL Admins`. I myself was part of this exclusive gang (r/humblebrag). This is one of the few rooms which are air-conditioned and fully accessible to students. People used to come sit there and study, hang out, have meetings, do their work or anything they wanted in cold comfort. We also used to open it up to conduct events and what-not.

Especially during the last few weeks of college, a lot of us final years used to sit there and work on our projects. I have a lot of nice memories conducting events, watching movies, staying up all night for a hackathon, and just talking to people in SSL.

{{<image src="ssl.jpeg" caption="a picture of the lab from my last day as admin">}}

### Athena

Aside from the "key duty", we as admins also had root access to all the PCs and our beloved server, Athena (top right corner in the image above). We were expected to maintain these systems and was responsible for hosting various college services. It was a great learning experience being given full access to a proper server and pulling out the display in the rack to physically connect to the server felt very cool.

We were sometimes even tasked with installing software on all the potato PCs in the lab which would always end up in some issue. We would always spend a lot of time trying to get it to work using [clusterssh](https://github.com/duncs/clusterssh), sometimes taking even more time than it would have taken to physically go to each of the systems and install it. Fun times!

## FOSSMeet'23

[FOSSMeet](https://fossmeet.net) is an annual Free and Open Source Software conference that is conducted by the [FOSSCell](https://fosscell.org/) of NITC. I was a part of this group and took part in the organisation of this event in 2023. This was the first FOSSMeet after the long COVID hiatus. All the seniors who had experience conducting the event had already graduated and left, leaving us to start from a blank slate.

The event turned out really well, and we got to interact with a bunch of active FOSS people from Kerala and beyond. The core team (led by {{<person name="Pavithra" url="https://pavithra.dev" text="CSEA Secretary" picture="https://pavithra.dev/assets/me.jpeg">}} and {{<person url="https://linkedin.com/in/abinlatheef/" name="Abin"  text="FOSSCell Secretary">}}) really managed to make a lot happen on a limited budget. {{<person name="Naveen" url="https://naveensd.com" text="fellow fossy boy">}} has a [post on his blog](https://naveensd.com/logs/fossmeet23/) which delves into more details about the happenings of the event.

{{<image src="fmorg.webp" caption="most of the organisers of FOSSMeet'23" >}}

Though I wasn't super involved in the organisational part of this, I did take part during the runtime. I was involved in the panel discussions and workshops and was in charge of _place.git_, an event that me and {{<person name="Joel" url="https://joelmc.com" text="fossmeet core">}} came up with, which was basically just everything [r/place](https://en.wikipedia.org/wiki/R/place) was, but on a [GitHub repo](https://github.com/fossmeet/place). This whole thing was hacked together the night before the event and actually turned out half decent.

The night spent in SSL just talking to the speakers and participants, attending the talks, chasing behind a random cat that got into the venue are a few memories I think I'll remember from this event.

## Tech Team

Ragam and Tathva are two annual fests that happen in our college. I joined the tech team in 2020 (my second year) and took up the responsibility of building the backend for the website on a whim. I had no experience in building any web stuff and only took it up because I would be forced to work on CSS stuff otherwise.

Building a proper production backend with no security issues and payments and stuff with zero context was actually quite overwhelming. With the help of the tech team seniors who sat with me and patiently explained to me even the silliest of issues, I eventually did figure it out (see: [projects]({{<relref "/projects/tathva-ragam-backends">}})). This was my first experience with hosting, backups, server maintenance and cloud stuff in general. This would eventually form the basis for me becoming an SSL Admin and a bunch of stuff I would do in the coming years.

I stayed on with the tech team for the next two years as "the backend guy". It felt really nice being involved in something at this scale and see your code working even with thousands of unique users. Once college reopened, I got to be involved in the working of an offline Ragam as well. This of course, had completely different requirements and involved a lot more moving parts. I spent a lot of the first day running around setting out fires and resolving bugs. Definitely would recommend the experience!

## Rajpath RECalls

Rajpath RECalls, started by {{<person name="Vysakh" text="passionate ass dude" url="https://linkedin.com/in/vysakh-premkumar/">}}, aimed to setup a campus radio for our college during the _dark days_ (COVID). He gathered people from our batch to run the shows, design posters, create content, manage the tech infra (where I came in) and all the other work that goes into running a web radio. And we made it work! I built an Android app (see: [projects]({{<relref "/projects/rajpath-recalls">}})) which would allow users to tune into the radio.

![screenshots]({{<relref "/projects/rajpath-recalls">}}banner.png)

I'm quite proud of how the app turnt out in terms of its design and functionality. It went out to the Play Store and hit a 1000+ downloads, the highest of any of my mobile apps. The radio ran for a while, with a bunch of shows and a reasonable audience for each of them.

We peaked when Anuragam, the annual Valentine's Day event at NITC, happened on our radio in 2022. The app hit a 1000 concurrent users, our servers got rate-limited and we ended up redirecting users to other streams of the event. The event turned out to be a success, and I suppose the platform got legitimized as well. However, after this, the team generally started losing interest in Rajpath RECalls, college re-opened, and people got busy with their own lives. Vysakh tried really hard to keep it going, but sadly, it eventually just faded into obscurity. Atleast we went out with a bang!

## Stallman

This might be seem pretty random, but I got to spend a day with {{<person name="Dr. Richard Stallman" text="gnu man" url="https://stallman.org/" picture="https://www.gnu.org/graphics/gnu-head.jpg">}}, founder of the [GNU](https://gnu.org) project and the [Free Software Foundation](https://fsf.org). One of the big names when it comes to the GNU/Linux community.

He had come for a lecture as part of Tathva, and wanted to have lunch with a few students before his journey back. So, me and a couple of friends went to receive him with a bunch of Kerala food, and spent the afternoon talking to him. I am really glad I got to have the experience, but all I can say is that the proverb "never meet your heroes" made a lot of sense to me that day!

## expOS, exPL and NITCbase

[expOS](https://exposnitc.github.io) and [exPL](https://silcnitc.github.io) were projects built within NITC that guides a student through the implementation of an elementary operating system and compiler respectively. These are available as the electives _OS Lab_ and _Compiler Lab_. I'd personally say that these are some of the best courses that NITC has to offer. I loved the self-paced nature of these courses with zero instructional hours.

Building these out was pretty rewarding, and I learnt a lot about the fundamentals of how the Linux kernel is structured and just the various components of what makes a kernel. I decided to do _Compiler Lab_ in Haskell (see: [projects]({{<relref "/projects/toy-compiler">}})), which was my first foray into functional programming. I really enjoyed grinding to finish these projects as fast as I could. Also, it was during the implementation of expOS that I spent a large amount of time on my Linux partition, which finally convinced me to daily-drive Linux, unlocking a whole new hobby.

I was so impressed by these courses that I decided that I wanted to be part of them too. In my final year, I joined the developer team for both of the above projects and also joined under {{<person name="Murali" url="https://people.cse.nitc.ac.in/muralikrishnan/" >}} Sir to build NITCbase (see: [projects]({{<relref "/projects/nitcbase">}})), a third entry in the series, which would guide a student through the implementation of a relational DBMS. Sitting in the SSL, reviewing and writing the documentation for NITCbase, was a really memorable part of my final semester.

{{<image src="nitcbase.jpeg" caption="me and Murali Sir working on NITCbase" >}}

## Freelancing and Internships

In the vacation after second year, a bunch of us from the tech team applied to and started freelancing at [Storilabs](https://storilabs.com/). Though I'd done a few odd jobs here and there, this was my first _job_ job. I learned React for the job and took up work in a team that built custom applications for Shopify merchants. This is also where I started using Git in a more advanced capacity and started collaborating with people on a large project.

It was from Storilabs that I got my first salary, with which I bought myself a mechanical keyboard and a pair of headphones. Spending my own money to buy myself luxuries was a joy I'm sure is relatable to most people. I worked there for a few months, until the online semesters came to an end and I found myself back in college. Academic work along with the tech work for the offline Ragam'22 was too much to manage along with a job, because of which I had to say my farewell to Storilabs.

In the following summer, I moved to Hyderabad and interned at [D.E. Shaw](https://www.deshawindia.com/). I stayed at a hotel for two months and got to work at a proper office with a standing desk, massive monitors and attended meetings, met deadlines, interacted with a lot of people, and in general, had a blast working there. I also have a lot of great memories from my time chilling with the other interns in and out of the office.

## NITC Mail

I feel like I cannot write a blog post about my time at NITC without mentioning the organisational mail. I cannot measure the sheer amount of time I've spent reading through silly mail threads, and random drama that pops up in college that prompts mails to all students. I sure am going to miss these mails now that I'm out of college, but I do hope the NITC mail remains just as it is in all it's glory for years to come

## Cali

Calicut, or Koഴikode, is the beloved city where our college is located. Over the course of the offline times, I'm sure all of have been back and forth from Calicut countless times. Every so often, this conversation comes up among me and my friends.

{{< admonition quote "Going Out" true >}}
Let's go to Kozhikode.

> What do we do in Kozhikode?

The beach

{{< /admonition >}}

Ah, the beach! The main attraction of the city. I'm convinced that almost everything that happens in Kozhikode is in and around the beach. The ice shaving thing we get there, the beautiful sunsets, the events that happen there, the amazing restaurants nearby are all memories that I will fondly remember as I leave this city. One of my favourite memories is the time that we went out to the beach at night, bought shawarma, and sat down by the beach and ate it with the gentle lull of the waves in our ears. We even ended up naming a group chat after this ("shawarma by the beach" gang represent🤙).

## Parting Thoughts

I'm sure this is a very common feeling, but yeah, I too felt like my four years in college just went by in a flash. I like to think I've come out of it a lot wiser than I went in. In my first year, I used to be in awe when I heard the seniors speak about CS stuff. I couldn't understand a word that they were saying. I've strived towards being someone like those seniors, and as I leave college, I think that I have become that person. I am proud of myself for everything I've learnt and I'm grateful to all that people that helped me learn these lessons; be it in life or in computer science.

I don't think I'm cut out to give any advice, but if there's one thing at all that I would say to someone who's starting out at NITC, it would be to find the right people. NITC has so many amazing people, with so many varying skills, and interests. Just spending time with these people and the ideas they bring really round you out as a person and inspire you to improve yourself. Whatever your interests are, there'll be someone with something to teach you about it. And with those words and a heavy heart, I hereby close my chapter at NITC!
