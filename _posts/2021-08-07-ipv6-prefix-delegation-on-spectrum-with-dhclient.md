---
date: 2021-08-07 15:10:09-07:00
excerpt: Information on how to request and maintain an IPv6 /64 prefix on the Spectrum cable network
excerpt_standalone: true
layout: post
title: IPv6 Prefix Delegation on Spectrum with dhclient
---
A few years ago, Spectrum (a US cable company formed from the combination of Charter and Time Warner Cable) started offering IPv6 Prefix Delegation (PD) /64s.  The device connected to the cable modem will normally get an individual global dynamic IPv6 address via Router Advertisement, but Prefix Delegation is essentially the ability to request an entire network to be routed to you.

I used to live in Reno in a formerly Charter network, but recently moved to Southern California in a formerly Time Warner network, so I'm confident this information applies to all Spectrum regions.  The `dhclient` invocation should work for any provider which supports Prefix Delegation, but the lease behavior I describe is probably not universal.

Here's the systemd `dhclient6-pd.service` file on my router, a Raspberry Pi 4 connected directly to the cable modem.  Replace `eext0` with your external interface name.
```systemd
[Unit]
Description=IPv6 PD lease reservation
Wants=network-online.target
After=network-online.target
StartLimitIntervalSec=0

[Service]
Restart=always
RestartSec=30
ExecStart=/sbin/dhclient -d -6 -P -v -lf /var/lib/dhcp/dhclient6-pd.leases eext0

[Install]
WantedBy=multi-user.target
```

Once running, `dhclient6-pd.leases` should give you something like this:

```
default-duid "\000\001\000\001#\225\311g\000\006%\243\332{";
lease6 {
  interface "eext0";
  ia-pd 25:a3:da:7b {
    starts 1628288112;
    renew 1800;
    rebind 2880;
    iaprefix 2600:6c51:4d00:ff::/64 {
      starts 1628288112;
      preferred-life 3600;
      max-life 3600;
    }
  }
  option dhcp6.client-id 0:1:0:1:23:95:c9:67:0:6:25:a3:da:7b;
  option dhcp6.server-id 0:1:0:1:4b:73:43:3a:0:14:4f:c3:f6:90;
  option dhcp6.name-servers 2607:f428:ffff:ffff::1,2607:f428:ffff:ffff::2;
}
```

So now I can see that 2600:6c51:4d00:ff::/64 is routable to me, and can set up network addresses and services.  `dhclient` could be set up to run scripts on trigger events, but in this current state it just keeps the PD reservation.

But... `max-life 3600`?  Does that mean I'll lose the PD if `dhclient` doesn't check in within an hour? What if I have a power outage?  Yes, you will lose the PD after an hour if `dhclient` isn't running... for now.  After a few renewals, the far end will trust that your initial PD request wasn't a drive-by, and will up the period from 1 hour to 7 days, and `dhclient6-pd.leases` will look like this:

```
default-duid "\000\001\000\001#\225\311g\000\006%\243\332{";
lease6 {
  interface "eext0";
  ia-pd 25:a3:da:7b {
    starts 1628288112;
    renew 1800;
    rebind 2880;
    iaprefix 2600:6c51:4d00:ff::/64 {
      starts 1628288112;
      preferred-life 3600;
      max-life 3600;
    }
  }
  option dhcp6.client-id 0:1:0:1:23:95:c9:67:0:6:25:a3:da:7b;
  option dhcp6.server-id 0:1:0:1:4b:73:43:3a:0:14:4f:c3:f6:90;
  option dhcp6.name-servers 2607:f428:ffff:ffff::1,2607:f428:ffff:ffff::2;
}
lease6 {
  interface "eext0";
  ia-pd 25:a3:da:7b {
    starts 1628291743;
    renew 300568;
    rebind 482008;
    iaprefix 2600:6c51:4d00:ff::/64 {
      starts 1628291743;
      preferred-life 602968;
      max-life 602968;
    }
  }
  option dhcp6.client-id 0:1:0:1:23:95:c9:67:0:6:25:a3:da:7b;
  option dhcp6.server-id 0:1:0:1:4b:73:43:3a:0:14:4f:c3:f6:90;
  option dhcp6.name-servers 2607:f428:ffff:ffff::1,2607:f428:ffff:ffff::2;
}
```

(The last `lease6` is the most recent lease received.)

As far as I can tell, this 7 day PD can be renewed indefinitely; I was using the same network for nearly 2 years.  But be warned: `max-life` is final.  If you have a misconfiguration and `dhclient` doesn't check in for a week, Spectrum will release your PD immediately after 7 days and your client will receive a completely new /64.

---

Since this is fresh in my mind from setting up my new home, here are a few things to set up on your core router, but this is not meant to be an exhaustive IPv6 Linux router guide.

The external interface automatically gets a global dynamic v6 address; as for the internal interface, while you technically don't need a static address thanks to link-local routing, in practice you should give it one.  Here's my `/etc/systemd/network/10-eint0.network`:

```
[Match]
Name=eint0

[Network]
Address=10.9.8.1/21
Address=2600:6c51:4d00:ff::1/64
Address=fe80::1/128
IPv6AcceptRA=false
IPForward=true
```

You'll also want an RA daemon for the internal network.  My `/etc/radvd.conf`:

```
interface eint0 {
  IgnoreIfMissing on;
  MaxRtrAdvInterval 2;
  MinRtrAdvInterval 1.5;
  AdvDefaultLifetime 9000;
  AdvSendAdvert on;
  AdvManagedFlag on;
  AdvOtherConfigFlag on;
  AdvHomeAgentFlag off;
  AdvDefaultPreference high;
  prefix 2600:6c51:4d00:ff::/64 {
    AdvOnLink on;
    AdvAutonomous on;
    AdvRouterAddr on;
    AdvValidLifetime 2592000;
    AdvPreferredLifetime 604800;
  };
  RDNSS 2600:6c51:4d00:ff::1 {
  };
};
```

And a DHCPv6 server.  My `/etc/dhcp/dhcpd6.conf`, providing information about DNS and DHCP-assigned addressing (in addition to the RA autoconfiguration):

```
default-lease-time 2592000;
preferred-lifetime 604800;
option dhcp-renewal-time 3600;
option dhcp-rebinding-time 7200;
allow leasequery;
option dhcp6.preference 255;
option dhcp6.rapid-commit;
option dhcp6.info-refresh-time 21600;
option dhcp6.name-servers 2600:6c51:4d00:ff::1;
option dhcp6.domain-search "snowman.lan";

subnet6 2600:6c51:4d00:ff::/64 {
  range6 2600:6c51:4d00:ff::c0c0:0 2600:6c51:4d00:ff::c0c0:ffff;
}

host workstation {
  host-identifier option dhcp6.client-id 00:01:00:01:21:37:85:10:01:23:45:ab:cd:ef;
  fixed-address6 2600:6c51:4d00:ff::2;
}
```
