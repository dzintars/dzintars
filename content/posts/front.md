---
title: "SPA Front page application"
date: "2024-02-19T18:33:05+02:00"
draft: false
tags: ["Web", "Redux", "Web Components", "Webpack", "Bazel"]
categories: ["Projects"]
---

## About the project

It's a SPA kind of web application based on bunch of buzzword technologies, most
importantly Web Components.
The idea behind picking web components was that over time I will accumulate
bunch of reusable components which will be any framework agnostic. At time of
creating that project React was the only **framework** which didn't support web
components, but times might be changed since then.

Images

### TIL

Redux features

WebPack config organization

SMACSS/SASS (tried to inject SASS into templates)... it should be possible, but
not sure how good is that idea. Modern CSS is pretty great to work with.

Figma

### Mistakes and Challenges

It was challenging to get around the "good" project structure. For example, it
took some time until I found out about Redux "features".

I created entire authentication UI but later after some experience gained I
understand that that was a mistake because, ideally I should use some SSO
solution and in my case it will be **Keycloak**.

### Current status

I would say it's abandoned... for now...

It all stopped the point where I was trying to generate Go stubs and TypeScript
message types from Protobufs. I succeeded, but the process of that was not
streamlined because I was required to manually update bunch of things in order
to implement generated artifacts.
This is where I went for the Bazel. My early tests showcased that Bazel is great
for these kind of tasks and it is fast.
