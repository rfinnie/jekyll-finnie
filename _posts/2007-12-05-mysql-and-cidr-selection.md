---
date: 2007-12-05 21:44:00-08:00
layout: post
lj_import_url: http://fo0bar.livejournal.com/185275.html
lj_itemid: 185275
title: MySQL and CIDR selection
wp_id: 705
---
I really like MySQL (yes, that won't win me any brownie points in my circle of friends) for its balance between power and simplicity, but I do miss a few things compared to other databases such as PostgreSQL. Some of them, such as subselects and stored procedures, have been introduced, but what I really miss is the "inet" data type. You can do most of its functionality with regular ints (well, unsigned ints), but it takes a lot more work. This little guide is here mainly for my own reference, but hopefully others will find it useful.

First of all, let's define our reference tables. Again, remember to use unsigned ints, since 

<tt>inet_aton</tt> and <tt>inet_ntoa</tt> work in unsigned 32-bit integers.


<pre>
mysql> create table ip_table (ip int unsigned not null, primary key (ip));
Query OK, 0 rows affected (0.00 sec)

mysql> insert into ip_table (ip) values (inet_aton('192.168.0.5')), (inet_aton('192.168.0.13')), (inet_aton('192.168.0.200')), (inet_aton('192.168.0.127')), (inet_aton('192.168.0.128')), (inet_aton('192.168.0.0'));
Query OK, 6 rows affected (0.00 sec)
Records: 6 Duplicates: 0 Warnings: 0

mysql> create table network_table (network int unsigned not null, netmask int unsigned not null, primary key (network, netmask));
Query OK, 0 rows affected (0.00 sec)

mysql> insert into network_table (network, netmask) values (inet_aton('192.168.0.0'), inet_aton('255.255.255.128')), (inet_aton('192.168.0.0'), inet_aton('255.255.255.0')), (inet_aton('192.168.0.192'), inet_aton('255.255.255.224'));
Query OK, 3 rows affected (0.00 sec)
Records: 3 Duplicates: 0 Warnings: 0
</pre>

Which produces the following:

<pre>
mysql> select inet_ntoa(ip) as ip from ip_table;
+---------------+
| ip            |
+---------------+
| 192.168.0.0   |
| 192.168.0.5   |
| 192.168.0.13  |
| 192.168.0.127 |
| 192.168.0.128 |
| 192.168.0.200 |
+---------------+
6 rows in set (0.00 sec)

mysql> select inet_ntoa(network) as network, inet_ntoa(netmask) as netmask from network_table;
+---------------+-----------------+
| network       |         netmask |
+---------------+-----------------+
| 192.168.0.0   | 255.255.255.0   |
| 192.168.0.0   | 255.255.255.128 |
| 192.168.0.192 | 255.255.255.224 |
+---------------+-----------------+
3 rows in set (0.00 sec)
</pre>

Now, say I want to know all IPs in <tt>ip_table</tt> that fall within 192.168.0.0/25:

<pre>
mysql> -- ips within a network, given a network address and netmask (192.168.0.0/255.255.255.128 here):
mysql> select inet_ntoa(ip) as ip from ip_table where (ip & inet_aton('255.255.255.128')) = inet_aton('192.168.0.0');
+---------------+
| ip            |
+---------------+
| 192.168.0.0   |
| 192.168.0.5   |
| 192.168.0.13  |
| 192.168.0.127 |
+---------------+
4 rows in set (0.00 sec)
</pre>

Or, say I want to know all networks in <tt>network_table</tt> that 192.168.0.5 would fall within:

<pre>
mysql> -- networks that will contain a given ip (192.168.0.5 here):
mysql> select inet_ntoa(network) as network, inet_ntoa(netmask) as netmask from network_table where (inet_aton('192.168.0.5') & netmask) = network;
+-------------+-----------------+
| network     | netmask         |
+-------------+-----------------+
| 192.168.0.0 | 255.255.255.0   |
| 192.168.0.0 | 255.255.255.128 |
+-------------+-----------------+
2 rows in set (0.00 sec)
</pre>

Also, here are some functions for basically doing everything you can in the <tt>ipcalc</tt> utility. Yes, you probably should do these calculations in your native programming language, but these might be handy somewhere:

<pre>
mysql> -- network address for a given ip and netmask (192.168.0.5/255.255.255.128 here):
mysql> select inet_ntoa(inet_aton('192.168.0.5') & inet_aton('255.255.255.128')) as network;
+-------------+
| network     |
+-------------+
| 192.168.0.0 |
+-------------+
1 row in set (0.00 sec)

mysql> -- broadcast address for a given ip and netmask (192.168.0.5/255.255.255.128 here):
mysql> select inet_ntoa(inet_aton('192.168.0.5') | (inet_aton('255.255.255.128') ^ (power(2, 32) - 1))) as broadcast;
+---------------+
| broadcast     |
+---------------+
| 192.168.0.127 |
+---------------+
1 row in set (0.00 sec)

mysql> -- first "usable" ip for a given ip and netmask (192.168.0.5/255.255.255.128 here):
mysql> select inet_ntoa((inet_aton('192.168.0.5') & inet_aton('255.255.255.128')) + 1) as first_usable;
+--------------+
| first_usable |
+--------------+
| 192.168.0.1  |
+--------------+
1 row in set (0.00 sec)

mysql> -- last "usable" ip for a given ip and netmask (192.168.0.5/255.255.255.128 here):
mysql> select inet_ntoa((inet_aton('192.168.0.5') | (inet_aton('255.255.255.128') ^ (power(2, 32) - 1))) - 1) as last_usable;
+---------------+
| last_usable   |
+---------------+
| 192.168.0.126 |
+---------------+
1 row in set (0.00 sec)

mysql> -- number of "usable" ips for a given netmask (255.255.255.128 here):
mysql> select inet_aton('255.255.255.128') ^ (power(2, 32) - 2) as num_usable;
+------------+
| num_usable |
+------------+
| 126        |
+------------+
1 row in set (0.00 sec)

mysql> -- wildcard for a given netmask (255.255.255.128 here):
mysql> select inet_ntoa(inet_aton('255.255.255.128') ^ (power(2, 32) - 1)) as wildcard;
+-----------+
| wildcard  |
+-----------+
| 0.0.0.127 |
+-----------+
1 row in set (0.00 sec)

mysql> -- netmask for a given cidr (/25 here):
select inet_ntoa(power(2, 32) - power(2, (32 - 25))) as netmask;
+-----------------+
| netmask         |
+-----------------+
| 255.255.255.128 |
+-----------------+
1 row in set (0.00 sec)

mysql> -- cidr for a given netmask (255.255.255.128 here):
select 32 - bit_count(power(2, 32) - inet_aton('255.255.255.128') - 1) as cidr;
+------+
| cidr |
+------+
| 25   |
+------+
1 row in set (0.00 sec)
</pre>
