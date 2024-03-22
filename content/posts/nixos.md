---
title: "NixOS vs Ansible"
date: "2024-02-28T12:55:08+02:00"
draft: false
tags: ["NixOS", "Ansible"]
categories: ["Linux", "Ansible"]
---

{{< note title="My opinion on this is still not stable or true and I might change my mind later when I will really try NixOS." >}}

I see the constant hype around the NixOS and one of the main selling points is
its reproducibility. Under the Ansible, you configure only whats in your
playbooks. And it's easy to mess around the system bypassing the Ansible. In
NixOS that's not the case. If you make any add-hock changes on NixOS and then
run `nixos-rebuild` all your crafted changes will be gone because they are not
in your NixOS config.
But this is what I like about the Ansible. I can mess around the system, use/evaluate
my tweaks until the next OS reinstall or to put in my playbooks only those parts which I really like.
Next time I will install OS I will get only my particular features. I don't need
to be afraid to loose some of my temporary tweaks by accidental `nixos-rebuild`.
I believe, you could do the same with NixOS, but... `nixos-rebuild` sounds too
dangerous to have there (as of my current understanding).
As of my understanding now, NixOS is more suitable for server setups, where the
all components are well known and you truly need 100% reproducibility.

The feature of NixOS I might like is the **rollback**. You can't do that with
the Ansible, thou... I'm not sure would I use it. If I can reproduce entire
system just with a single Ansible play, then I prefer to do clean install to be
sure, that there are no any leftovers.
