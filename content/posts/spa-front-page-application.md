---
title: "SPA Front page application"
date: "2024-02-19T18:33:05+02:00"
draft: false
tags: ["Web", "Redux", "Web Components", "Webpack", "Bazel"]
categories: ["Projects"]
---

## About the Project

It's a SPA-style web application based on a bunch of buzzword technologies, most importantly **Web Components**. The idea behind choosing web components was to accumulate a bunch of reusable components that would be framework-agnostic over time. At the time of creating the project, React was the only framework that didn't support web components, but times might have changed since then.

{{< note title="TODO: Add some images" >}}

### TIL (Today I Learned)

**Redux** features: Thinking in terms of "features" helped me come up with a better code structure.

**Webpack** config organization: Not a significant challenge, but I found a nice finding that I can compose different configs depending on the environment. However, Webpack doesn't integrate well with **Bazel** because it's hard to get Hot Module Replacement (HMR) working with the Bazel runner and to preserve the app state. I'm considering going with a full deployment cycle; Bazel should be able to run a container in a few seconds. But I'm not sure how to deal with the app state if I still use Redux. Local storage? WebSocket?

**SASS/SMACSS**: I tried to inject SASS into **Lit** (LitElement) templates; it should be possible, but I'm not sure how good of an idea it is. Modern CSS is pretty great to work with.

**Figma**: An amazing tool! Premium is worth every penny as it allows you to properly structure large projects. At that time, I was missing design tokens, a.k.a. variables.

P.S. I'm happy that Adobe deal failed.

### Mistakes and Challenges

It was challenging to establish a "good" project structure. For example, it took some time until I found out about Redux "features". Also, currently, I'm not 100% sure if I want to go with a single store approach at all. I like Redux, but I'm not sure if it's the best solution for the kind of application I'm trying to work on. I'm looking into something more "reactive", maybe **RxJS**? I also looked into [redux-dynamic-modules](https://redux-dynamic-modules.js.org), which could be a backup "concept" if my "reactivity" approach does not work out.

I created an entire authentication UI, but later, after gaining some experience, I realized that it was a mistake because ideally, I should use some Single Sign-On (SSO) solution, and in my case, it will be **Keycloak** because it's a widely adopted (maintained) open-source project.

It is a bit challenging to come up with a user-friendly, ergonomic, and at the same time, "power user" UI. If you make it too user-friendly, there will be too much clicking around for power users. If you make it look clean, power users will not like it as they expect maximum screen real estate to be filled with useful data. Also, power users like "windows"; they are comfortable working with multiple modals and dialogs. But that is not "accessibility-friendly", nor is it easy to develop/maintain. So, my goal is to start from the bottom up. I will go for mainstream users with the "task-based UI" approach first: simple components that do one thing and do it well, with maximum composability. Then, I will create a "condensed" version of the same component and give power users the ability to configure the component version.

Also, I'm almost sure that a **SPA** is not the right solution there, and I should go with an **MPA** approach for long-term maintainability and scaling reasons.

How to combine "free to use" with "I, as a user, want something more than just free"? My idea is to give the tool for free to use; if the back office doesn't cost me anything other than development, then the user should be able to use the tool for free. But if they want collaboration features, backup storage, online access to some data sets, and so on, then they must go with the paid version, as that will cost me money to store, backup, and maintain. The challenge is how to enable a seamless transition from free to paid and back so that user data is "moved" from local storage to the servers or vice versa.

### Current Status

I would say... it's temporarily abandoned...

It all stopped at the point where I was trying to generate Go stubs and TypeScript message types from Protobufs. I succeeded, but the process was not streamlined because I had to manually update a bunch of things in order to implement generated artifacts. This is where I turned to Bazel. My early tests showed that Bazel is great for these kinds of tasks, and it is really fast.

### Plans

If I am able to recall my memory on that project, I might continue to work on it later. It wasn't that bad. I liked many things about it. I just need the "development platform" to run smoothly so that I don't need to think much about shipping. I want to automate everything I can.
