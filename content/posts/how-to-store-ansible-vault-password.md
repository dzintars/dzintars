---
title: "How to Store Ansible Vault Password"
date: "2024-03-27T13:18:12+02:00"
draft: true
tags: ["Ansible", "Vault", "Security", "KeePassXC", "YubiKey"]
categories: ["How To's"]
---

There are several ways to pass the Ansible Vault password.

- classical CLI prompt which requires manual intervention
- password file which should be stored somewhere accessible to the Ansible
- environment variable

Today I will tell about my latest approach.

I am using tool called `Direnv` which loads and unloads different environment
variables when you enter the directory.

Direnv uses `.envrc` files to read the variables from.
