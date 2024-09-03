---
title: "My Note Taking System"
date: "2024-02-24T14:10:25+02:00"
draft: false
tags: ["Neovim", "Sway"]
categories: ["Productivity"]
---

{{< note title="WIP. This post is not finished." >}}

## Why

For a long time I didn't care about the note taking too much, thou, I was
writing long MS Word documents with the ideas of the project, user stories and
other things.

It all changed when I switched to the Linux. Because Linux was pretty much new
to me, I had a lot to learn and remember. So, gradually I started to write my
little runbooks to document the steps I took to get to the point B. For example,
how do I create SSH keys. Or how do I configure my firewall. And so on.

## Touch Typing

Almost all of the "productivity'fluencers" never mentions the importance of the
touch typing to get into habit of note taking.
But in my opinion... that is the very first pill, you should take before getting
into note taking. If the typing is the pain for you, then you will not make any
notes, nor note taking system. No... you will take notes, but those will be just
occasional sessions. Not a real note taking.

It changes, when you are able to type just by looking at the screen. Without
hunting for the each letter on the keyboard. Even better, if you can train
yourself up to 150 words per minute (wpm), it means, you are able to type almost
at a speed of your thought.

## Ansible

Over time, after I played with custom runbooks and scripts, I started to write
every little system configuration task in Ansible. This gave me systematic space
to document my findings, to write some readme's etc.

## Notion, Roam-Research & Co

I tried all of them. And all of them felt... clumsy. They required too much
effort just to make a quick note. Like... opening new browser tab already felt
too distracting.

Then I found out Obsidian. Local files. I own my content. Reusable file format.
It ticked all checkboxes.
The only issue was - it's an Electron app. And somewhat that felt too...
clumsy.

As my note taking experience evolved, I finally found out about `ZK` and `ZK-Nvim`.
And my puzzle was solved. ZK is Obsidian "compatible". Which means... all my
notes are kept in a single Git repository. I can open them with Obsidian if
required, but usually I just use Neovim for that.

I have quick keybinds in my Sway config to open my ZK in a floating window. So..
my notes always are there right at my fingertips and I don't even need to reach
for the mouse.

## Neovim

## Custom Keyboard

![Keeb.io Iris](/images/keebio-iris-kat-blank.jpeg)

## Keyboard centric workflow

Over time I moved to keyboard-centric workflow. It even got better once I
learned proper touch typing.

## i3, Sway and Tmux

## Project Readme's, GitHub issues, Wikis

Before I developed my own "note taking system/pattern", I tried to use
everything else. First it was MS Word. Then it was Google Docs. Then I had my
own Confluence server instance. But at that time it was too clumsy to maintain.
I didn't knew nothing about automation. I did everything by hand. Heck... I even
didn't knew Linux all that much. It was just some VM which I somehow magically
managed to setup and install Confluence on it. I think it was even Virtualmin UI
I used to manage it. So... as more and more system resets came into my life,
eventually I lost all of my Confluence knowledge base. Including relevant Jira
tasks. At that point it was clear, that I need some simpler way to do my
writings.

So after that experience, I tried out GitHub issues, Wiki's, Readme's, projects, pages...
It was simpler... I don't need to worry about hosting and backups. But you need
to be mindful about what you are putting out in the internet. Most of my stuff
was public.

## Flat File System

In the Wild Wild Web most of the cases I see people trying to come up with some
file structure for their notes. What directories to use? How should I tag my
notes? How should I name my files. Etc.
I came to realization... that no matter what file structure you will develop, it
always will get in your way. Should I place this note in the the "recipes"
directory or in my "health" directory? And so on. So... the best file structure
it no file structure at all. And this is how all of my thousand+ notes are
structured. Just 2 flat directories - "daily" and "base". And all of my note
files are named like `202310081951.md`. Name of this file never changes. I never
use file tree or explorer to find my notes. ZK handles (indexes) markdown front
matter. This is where intellisense comes from. If at any time my note changes
its content, I can tweak the note title accordingly without renaming entire
file. For me, finding and renaming file is too expensive. By using right tools,
my note management is super simple. None of my internal note links breaks. I can
use file paths in any external applications if required. Markdown is super
versatile and compatible format. It can be consumed by plethora of other tools.
It can be batch processed by scripts.

Also, I think it is worth noting, that I don't write "creative" content. 99.99%
of my notes are just a reminders of some commands, tricks, errors, etc. I might
say - a static content. So... for this reason my file organization works really
well for me.
But if I would be in the more creative way of things... this "file system" might
not work.

## Next steps

Automate publishing of my internal ZK to my Hugo blog.
