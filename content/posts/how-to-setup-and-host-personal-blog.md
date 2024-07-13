---
title: "How to Setup and Host Personal Blog for free with GitHub and Hugo"
date: "2024-07-13T23:49:42+03:00"
draft: true
tags: ["Hugo"]
categories: ["How To's"]
ShowToc: true
---

So... you are thinking about your own blog for a long time. But... it all felt
complicated, required too much time and you were even not sure, what would you
write there.

I can't help you with the "what to write" part... but, I can help to share my
experience about setting this blog up and hosting it for free.

I will try my best to write it a newbie friendly as I can, but it might be
challenging a bit as my own system and workflow is highly customized and I might
forgot that I have one or the other thing configured to make it work. And thus,
I migh write the workflow which does not work for you. If that is the case,
please feel free to ask for clarification in the GitHub issues. Just press the
"Suggest Changes" at the top of this article and open an
[Issue](https://github.com/dzintars/dzintars/issues).

## Linux or Windows

I am the Linux user.
So things are quite simple for me. But in case if you are an Windows user,
I strongly recommend you to use WSL (Windows Subsystem for Linux).
Just launch WSL and execute Linux commands in terminal.

## GitHub/Git repository

I will assume, that you know what the [GitHub](https://github.com) is. If I'm
wrong, head towards it and create your own account.

We will use free GitHub services to automate blog publishing and hosting.

First, you need to create the repository, what is named after your GitHub
username. For example, my GitHub user name is `dzintars`. So I created new
repository called `dzintars`. The link to that repository will look like
[https://github.com/dzintars/dzintars](https://github.com/dzintars/dzintars).
This is "special" GitHub repository with some extra capabilities.

## SSH Keys

I don't know, can you still use plain text password to clone your own repository
from GitHub as there was some security improvements made by GitHub, but I will
teach you how to use more secure authentication mechanism for GitHub.

Most likely, you already have all the tools installed and running on your Linux
or WSL instance.

You now need to generate your secret keys. Execute this command. Modify the
parts you want to modify. This is not dangerous command in any way. It simply
will create 2 text files in your `~/.ssh` directory.

```bash
ssh-keygen -o -a 100 -t ed25519 -f ~/.ssh/id_ed25519 -C "your-name@your-machine-name-or-brand"
```

`-C` means "comment". You can write anything there, but I would advice you to
write who is using this key and from what machine. It is good practice not to
reuse one single SSH key for all SSH related activities. Instead, you use one
key for GitHub. Other key for your server. Third key for something else. In case
if one key got leaked (somebody got your SSH private key), then your "blast
radius" is much smaller.

You can set the password for your SSH keys to made these keys unusable without
the password. Or you can just hit the `Enter` twice, to create non-protected SSH
keys. It's up to you. I personally don't use passwords for all of my keys. Only
for those, which are used for higly secure environments. For now... don't set
the password. You can create other password-protected keys later. Password
handling will complicate your first time blog setup experience a bit.

So... in your `~/.ssh` directory you now have 2 files:

```txt
id_ed25519
id_ed25519.pub
```

`id_ed25519.pub` is your public key. You can share it, publish it... it holds no
secrets. But the `id_ed25519` is your password. If anyone will gain access to
this file, your GitHub account will be at danger. So... **NEVER** expose this
file to anyone. If you feel that you leaked it, create new key and change your
public key in all the places you were using your previus key. In our case, it
GitHub only.

In the wild you might see many examples who uses `ssh-keygen -t rsa -b 2048
bla-bla-bla` command. `RSA` is simply older encryption algorithm. We are using
more modern version there and it is supported by the GitHub as well.

Now... open your `id_ed25519.pub` file in any text editor (just don't open it
with MS Word :smiley: ) and copy all the text from it. `Ctrl + a` to select all
and `Ctrl + c` to copy it into clipboard. You can use Notepad, Notepad++, Visual
Studio Code.

Then go to your GitHub profile settings and under SSH Keys section add new entry
and `Ctrl + v` paste your SSH public key from your clipboard.

![Image of RSA SSH public key pasted into GitHub add SSH key form](/images/ssh-key-github-add.png "Paste your SSH key. For ED25519 it will be shorter.")

OK... now your GitHub account have 2 types of authentication - password and SSH
key.

![Image of RSA SSH key added to the list of all GitHub account SSH keys](/images/ssh-key-github-added.png "The list of GitHub SSH keys.")

_P.S. I made example keys just to create these screenshots._

You use your account password just to log into GitHub web page. And you use
your SSH key to "copy" and "paste" your Git repositories. In GitHub language it
is called to "clone/pull" and "push". You do `git clone` when you want to get
your remote (GitHub) repository on your PC for the first time. You use `git pull`
when you just want pull the changes from the remote GitHub repositoy which might
be or might be not there. For example, you used GitHub Web editor to make some
changes in your content directly in your GitHub repository. To get them locally
into your existing local repository at `~/code/your-github-username/your-github-username`
you use `git pull`. For now (there are other commands as well).
And you use `git push` when you want to publish your locally made changes to the
GitHub.
There are few more commands you will need... but I will tell you about them
later.

So... for now execute these 2 commands:

```bash
mkdir -p ~/code/your-git-username && cd ~/code/your-git-username
git clone git@github.com:your-git-username/your-git-username.git
```

Now, you can see new directory called `your-git-username` in your
`~/code/your-git-username` directory. Open it.
You might see just a single `README.md` file. Probably you need to turn on "hidden
files". I'm not sure, how the Windows display hidden files and directories. In
the Linux world, everyting that starts with `.` (dot) is called hidden file or
directory and by default is not displayed in various tools.

So in your `~/code/your-git-username/your-git-username` you must see `.git`
directory. This indicates that this entire
`~/code/your-git-username/your-git-username` directory is your actual "git repository".

Great! At this point, we have no blog. But we have a "spot" where to start
create magic! :slightly_smiling_face:

## Branches

Because your `main` branch will be used for your public GitHub profile page, we
need to create separate git branch where we will keep our blog. Basically... we
will have two "spaces" where to keep the stuff.

Create new branch by executing this command:

```bash
cd ~/code/your-git-username/your-git-username && git checkout -b blog
```

You can change `blog` to anything you want to call your branch.

## Hugo installation

## Hugo configuration

## Theme

## Emoji's

`hugo.yaml`:

```yaml
enableEmoji: true
```

## GitHub Actions

## Typical Workflow

## Domain

You can setup your own personal domain for your blog.
