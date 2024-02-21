---
title: "Websockets and Kafka"
date: "2024-02-21T19:46:43+02:00"
draft: false
tags: ["Websocket", "Kafka", "CQRS"]
categories: ["Projects"]
---

## Why

I had this idea about the highly reactive collaborative web application.
After messing around I ended up with PoC which consisted of bunch of Go services
and on top of that I somewhat implemented CQRS architecture.

## How

[front](https://github.com/dzintars/front)
[wss](https://github.com/dzintars/wss)
[srp](https://github.com/dzintars/srp)

General idea was that websocket is used as protocol. Custom message format is
created and synchronized between TypeScript and Go with help of Protocol
Buffers, gRPC and few gRPC extensions. But this process is bit cumbersome and
this is where I went off-road into Bazel and DevOps...

Web produces and streams events to Kafka consumers. Once data are processed some
messages are broadcasted over websocket to the application. This
basically enables fast real-time collaboration in application.

## Challenges

Little to almost none of the content in the Internet to learn in depth about
these concepts. Especially if you are newbie like me and don't know even how to
ask the question. And if you do ask, then typical answer is

> you don't need this.

But... I didn't want to argue with some random person in the internet whether I
need or don't need this complexity. So... I gave up with asking these kind of
questions to the internet.

And my best source of learning became [GitHub Search](https://github.com/search)
