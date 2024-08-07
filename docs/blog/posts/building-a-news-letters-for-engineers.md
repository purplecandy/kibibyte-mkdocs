---
date: 2022-05-15
authors: [nadeem]
title: Building a newsletter for engineers (#1)
description: >
  Part 1 of building a newsletter for engineers
categories:
  - Indie Hacking
  - Ideas
comments: true
---

![Architecture diagram](https://res.cloudinary.com/kibibyte/image/upload/v1691933917/dc9e942c-605c-4325-b535-0c77d263ce27_qrbzjj.png)
It's been a long time since I have built something small, stupid, and fun. I'm planning to build a newsletter service for engineers that aggregates the data from popular engineering blogs and articles and gives a curated list of weekly updates. If you have used Feedly you know what I mean but rather than going to maintain stream content we are going to focus on blogs that primarily focus on system design, philosophy in programming, and various mature engineer discussions.

<!-- more -->

I do intend to take it production and have at least a handful of users actively using it. Throughout the development cycle, I plan to continuously publish articles on every milestone as a way of preserving notes for myself.

Our app will need several components. For a start, I intended to self-host and manage all the services.

**Storage**

Since we will store a relatively simple model that needs to maintain a couple of relations and should be capable of performing aggregations.

**APIs**

Anything that communicates requests without a system from server to client and vice-versa.

**CI/CD**

I intended to have a fully integrated pipeline that automatically deploys the latest version without/the least amount of downtime.

**Worker**

We will need a way to set up CRON and automatically parse the websites regularly and store the data in the database and also log any failed jobs. The action should also be performable at an instance as well.

**Frontend**

We need a frontend app that supports SSR and has features of modern web development. The primary reason for having SSR is to ship the least of JS to the user and intended to have a version of the system where we can render a hacker-news variant of the site without any javascript.

My goal is to deeply understand the CI/CD pipelines and tinker around with infrastructure and container deployment strategies. I have been using systems and part of teams that have abstracted a lot of things related to Infrastructure. As I wish to build more Indie Apps I want to build a foundation of core-stack that I usually work and play with.

Let's get started!
