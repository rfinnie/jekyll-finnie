---
date: 2020-06-03 12:40:55-07:00
excerpt: Safechain is a wrapper to load iptables chain rules in a safe, atomic, idempotent
  way.
excerpt_standalone: true
layout: post
title: 'Safechain: safe, atomic and idempotent iptables firewall management'
---
When I joined Canonical in 2012, we in IS had a number of choke firewalls which were literally just a server which ran iptables rules.  These firewalls would filter gigabits of traffic without blinking an eye, so it was sound from a technical perspective, but config management was a problem.

The general layout was a firewall.sh file which was run early on boot, which did initial setup and created network-specific chains.  The network-specific chain (which usually took the form net1_to_net2.sh) would be run from /etc/network/interfaces and would flush the chain and populate it.  This approach had two rather annoying flaws.

The first flaw was any sort of syntax error would leave the chain in a broken state.  Because of this, any updates to the firewall in the config management system (Puppet at the time) required a +2 to commit, instead of the normal +1.  Even then, chains would regularly break, leaving an SRE to scramble to cowboy in a fix while downtime occurred.

The second flaw is less obvious at first, but became a large problem later on.  As chains grew larger, it took longer to apply them.  A chain with thousands of rules could take seconds; tens of thousands of rules could take over a minute.  And since the first step was to flush the chain, this was time when a partially applied chain was in production.  SREs would start announcing when chain updates were being applied, chain files were rearranged so the most important rules were near the top, etc.

In early 2013, I wrote [Safechain](https://github.com/rfinnie/safechain), which ended up being one of the most proportionally simple-to-write-versus-headache-reducing scripts I had ever written.  In a nutshell, it takes the basic concept of chain-specific iptables firewall scripts, and makes it safe, atomic and idempotent.

A converted chain called "host_ingress" might look like this:

```bash
#!/bin/sh

set -e

. /etc/safechain/safechain.sh

# host_ingress chain preprocessing
sc_preprocess host_ingress

# Allow ICMP
sc_add_rule host_ingress -p icmp -j ACCEPT

# Allow all inbound traffic from the LAN
sc_add_rule host_ingress -i eth1 -j ACCEPT

# Allow certain services
sc_add_rule host_ingress -p tcp --dport 80 -j ACCEPT
sc_add_rule host_ingress -p tcp --dport 443 -j ACCEPT

# Allow SSH from trusted host
sc_add_rule host_ingress -s 10.2.8.3 -p tcp --dport 22 -j ACCEPT

# Drop all other traffic
sc_add_rule host_ingress -j LOG --log-prefix "BAD-host-in: "
sc_add_rule host_ingress -j DROP

# host_ingress chain postprocessing
# Goes live here if all went well
sc_postprocess host_ingress
```

In most cases, converting from iptables to Safechain was a matter of adding `sc_preprocess host_ingress`, `sc_postprocess host_ingress`, and changing all `iptables -A` to `sc_add_rule`.

`sc_preprocess` creates a temporary chain, which `sc_add_rule` adds to.  When `sc_postprocess` is run, a jump from the main chain to the temporary chain is added, the existing live chain is removed (including any references to it, at which point the new chain is now live), and the new chain is renamed to the live chain (including any references to it).  If an error occurs at any point in this process, the old chain will remain active, and it's impossible for a half-broken chain to be running.

This served us well for about five years, until we implemented a replacement declarative-based firewall system which used `ipset` under the hood.  But I still used Safechain at home, and got permission from Canonical to open source it.  Normally something like this would have been open sourced from the beginning, usually under the Ubuntu banner, but since it was used completely internally, nobody really thought about putting an LGPL header on it and posting it publicly.  Canonical doesn't really have an interest in Safechain since they don't use it for production anymore, so it's effectively "mine" now, but I still wanted to go by the book.
