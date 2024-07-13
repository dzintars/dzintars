---
title: "How to Store Ansible Vault Password"
date: "2024-03-27T13:18:12+02:00"
draft: false
tags: ["Ansible", "Vault", "Security", "KeePassXC", "YubiKey"]
categories: ["How To's", "Ansible"]
---

There are several ways to pass the Ansible Vault password.

- classic CLI prompt which requires manual intervention
- password file which should be stored somewhere accessible to the Ansible
- environment variable

Today I will tell about my latest approach.

I am using tool called [Direnv](https://direnv.net) which loads and unloads different environment
variables when you enter the directory.

Direnv uses `.envrc` files to read the variables from.

Typically, this file should be in your `.gitignore` so that you don't leak any
secrets.

But I went a little bit further and am exposing all of my environment variables.

For that to be secure, I am using KeePassXC for secret management.
My secrets databases, besides the master password are also protected with [YubiKey](https://www.yubico.com).

And I have exposed one specific directory for the Secret Service Integration.
Within that directory lives the development secrets which I want to expose for the
Linux secret service.

This gives me opportunity to do things like this:

```bash
export DOCKER_IO_PASS=$(secret-tool lookup Title docker-io-pass)
```

`secret-tool` is part of the `libsecrets` library. In this case `docker-io-pass`
is the actual title of the secret in the KeePassXC database.

Once I enter into particular directory, `direnv` will execute this command and
retrieve the actual secrets from the KeePassXC. So... no secrets are living in a
plain text on my workstation.

So... given such possibilities, I have simple `ansible.cfg` directive:

```ini
vault_password_file = ./scripts/ansible-vault-password.py
```

And it contains just a simple Python script:

```py
#!/usr/bin/env python

import os

passwd='secret-tool lookup ansible-vault project-password'
os.system(passwd)
```

`ansible-vault` is additional attribute name for particular KeePassXC secret,
and `project-password` is the value of that attribute. So... `secret-tool` is
"looking up" for the secret by the attribute name and its value. Not by the title,
path or something else.

You can also do things like this:

```bash
passwd='keepassxc-cli show -q -s -a password /path/to/vault.kdbx -y 2:12345678 /Path/To/Anible_Vault_Entry'
```

In this example, you can even specify ID of your YubiKey in case if you have
many of them. And we are using `keepassxc-cli` tool there which have some nice
QoL (Quality of Life) options.

Overall, I am using `secret-tool` and `keepassxc-cli` everywhere, where I need
to create or retrieve secrets dynamically. For example, you can use it to
generate Hashicorp Vault administrator password at HC Vault provisioning stage.
