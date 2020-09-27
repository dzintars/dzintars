### Hi there 👋

Currently busy with refactoring ["boilerplate" project](https://github.com/oswee/prime) to be managed by Bazel.
Whole project consists of various technologies, like Web Components (Lit Element), Redux,
WebSockets, Go services, Protobufs and more. Orchestrating those in isolation was kinda
hard so i decided to move everything into single monorepo and manage it via Bazel.
Early tests proves that this is good way to collect whole knowledge base.
But the primary issue which led me to this approach was inability to share WSS message definitions between Go and Typescript. Without Bazel I was forced to use Lerna to publish
type definitions as separate GitHub registry packages and then to import them in my
Redux app. And every time I change the Proto API, I must do this procedure again and again.
Monorepo solves this issue and forces me to learn at least some build system.

Before was learning graphical design and design systems.
Spent most of my day time in Figma and Axure and prototyping some of my project ideas.

All of my projects are just some rough drafts to test some ideas and to learn.

My current primary repositories are:

[Prime](https://github.com/oswee/prime)
Web Components (Lit Element), Redux, TypeScript, Go and Protobufs based web application.

[Frontend](https://github.com/dzintars/front)
Web Components (Lit Element), Redux and TypeScript based SPA frontend. (moving into Prime)

[Backend](https://github.com/dzintars/wss)
Really basic websockets backend API placeholder written in Go. (moving into Prime)

I also trying to automate my workstation setup with Ansible in:

[Infra](https://github.com/dzintars/infra)

### Music
[SoundCloud playlist](https://soundcloud.com/dzintars/sets/session)

<!--
**dzintars/dzintars** is a ✨ _special_ ✨ repository because its `README.md` (this file) appears on your GitHub profile.

Here are some ideas to get you started:

- 🔭 I’m currently working on ...
- 🌱 I’m currently learning ...
- 👯 I’m looking to collaborate on ...
- 🤔 I’m looking for help with ...
- 💬 Ask me about ...
- 📫 How to reach me: ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...
-->
