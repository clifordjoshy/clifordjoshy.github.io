---
title: "Tathva/Ragam Backends"
subtitle: "REST APIs for my college fests"
date: 2022-05-01T13:23:40+05:30
tools: ["strapi", "node", "postgres"]
resources:
  - name: "featured-image"
    src: "banner.png"
---

## Links
[:(fa-icon fa-brands fa-github): GitHub - Ragam21](https://github.com/clifordjoshy/ragam21-backend)&emsp;
[:(fa-icon fa-brands fa-github): GitHub - Tathva20](https://github.com/clifordjoshy/tathva20-backend)&emsp;
[:(fa-icon fa-brands fa-github): GitHub - Ragam22](https://github.com/Ragam22/backend)&emsp;

## About

I was involved in the tech teams of our college fests, Ragam and Tathva. Ragam'21 and Tathva'20 were conducted online and required a REST API capable of handling event registrations, submissions, payments, team management and other event-specific requirements.

Ragam'22 was (finally) offline and had a completely different set of requirements on top of the pre-existing design. It required the verification of attendees at the venues and events, hostel allocation, special registration conditions and the addition of an admin API.

**Stack**: All of these were made using [Strapi](https://strapi.io/), for the reason that it comes with an admin panel out of the box while maintaining the flexibility to model the endpoints. This was necessary for the management of events, workshops and lectures. PostgreSQL was the database.

**Deployment**: A DigitalOcean Droplet was allocated with additional block storage for the uploaded media. We handled more than 10,000 unique registrations on each of the websites.

## Personal Notes
This was the first time I worked with APIs, NodeJS, Strapi, PostgreSQL, deploying an app on the cloud and basically anything web. I'm really glad I took up this responsibility back during Ragam'21. It was majorly overwhelming and cause for me to freak out, but it was ever so worth it. Got to mess around with Linux servers, "pushing to prod" and a bunch of other stuff, all on the college dime.