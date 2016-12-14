---
author: Ryan Finnie
date: 2007-12-05 22:44:00
guid: http://www.finnie.org/2007/12/05/mysql-and-cidr-selection/
id: 705
layout: post
lj_import_url: http://fo0bar.livejournal.com/185275.html
lj_itemid: 185275
permalink: /2007/12/05/mysql-and-cidr-selection/
title: MySQL and CIDR selection
---
I really like MySQL (yes, that won't win me any brownie points in my circle of friends) for its balance between power and simplicity, but I do miss a few things compared to other databases such as PostgreSQL. Some of them, such as subselects and stored procedures, have been introduced, but what I really miss is the "inet" data type. You can do most of its functionality with regular ints (well, unsigned ints), but it takes a lot more work. This little guide is here mainly for my own reference, but hopefully others will find it useful.

<!--more-->First of all, let's define our reference tables. Again, remember to use unsigned ints, since 

<tt>inet_aton</tt> and <tt>inet_ntoa</tt> work in unsigned 32-bit integers.

> <tt>mysql> create table ip_table (ip int unsigned not null, primary key (ip));<br /> Query OK, 0 rows affected (0.00 sec)</p> 
> 
> <p>
>   mysql> insert into ip_table (ip) values (inet_aton('192.168.0.5')), (inet_aton('192.168.0.13')), (inet_aton('192.168.0.200')), (inet_aton('192.168.0.127')), (inet_aton('192.168.0.128')), (inet_aton('192.168.0.0'));<br /> Query OK, 6 rows affected (0.00 sec)<br /> Records: 6 Duplicates: 0 Warnings: 0
> </p>
> 
> <p>
>   mysql> create table network_table (network int unsigned not null, netmask int unsigned not null, primary key (network, netmask));<br /> Query OK, 0 rows affected (0.00 sec)
> </p>
> 
> <p>
>   mysql> insert into network_table (network, netmask) values (inet_aton('192.168.0.0'), inet_aton('255.255.255.128')), (inet_aton('192.168.0.0'), inet_aton('255.255.255.0')), (inet_aton('192.168.0.192'), inet_aton('255.255.255.224'));<br /> Query OK, 3 rows affected (0.00 sec)<br /> Records: 3 Duplicates: 0 Warnings: 0</tt>
> </p></blockquote> 
> 
> <p>
>   Which produces the following:
> </p>
> 
> <blockquote>
>   <p>
>     <tt>mysql> select inet_ntoa(ip) as ip from ip_table;<br /> +---------------+<br /> | ip |<br /> +---------------+<br /> | 192.168.0.0 |<br /> | 192.168.0.5 |<br /> | 192.168.0.13 |<br /> | 192.168.0.127 |<br /> | 192.168.0.128 |<br /> | 192.168.0.200 |<br /> +---------------+<br /> 6 rows in set (0.00 sec)</p> 
>     
>     <p>
>       mysql> select inet_ntoa(network) as network, inet_ntoa(netmask) as netmask from network_table;<br /> +---------------+-----------------+<br /> | network | netmask |<br /> +---------------+-----------------+<br /> | 192.168.0.0 | 255.255.255.0 |<br /> | 192.168.0.0 | 255.255.255.128 |<br /> | 192.168.0.192 | 255.255.255.224 |<br /> +---------------+-----------------+<br /> 3 rows in set (0.00 sec)</tt>
>     </p></blockquote> 
>     
>     <p>
>       Now, say I want to know all IPs in <tt>ip_table</tt> that fall within 192.168.0.0/25:
>     </p>
>     
>     <blockquote>
>       <p>
>         <tt>mysql> -- ips within a network, given a network address and netmask (192.168.0.0/255.255.255.128 here):<br /> mysql> select inet_ntoa(ip) as ip from ip_table where (ip & inet_aton('255.255.255.128')) = inet_aton('192.168.0.0');<br /> +---------------+<br /> | ip |<br /> +---------------+<br /> | 192.168.0.0 |<br /> | 192.168.0.5 |<br /> | 192.168.0.13 |<br /> | 192.168.0.127 |<br /> +---------------+<br /> 4 rows in set (0.00 sec)</tt>
>       </p>
>     </blockquote>
>     
>     <p>
>       Or, say I want to know all networks in <tt>network_table</tt> that 192.168.0.5 would fall within:
>     </p>
>     
>     <blockquote>
>       <p>
>         <tt>mysql> -- networks that will contain a given ip (192.168.0.5 here):<br /> mysql> select inet_ntoa(network) as network, inet_ntoa(netmask) as netmask from network_table where (inet_aton('192.168.0.5') & netmask) = network;<br /> +-------------+-----------------+<br /> | network | netmask |<br /> +-------------+-----------------+<br /> | 192.168.0.0 | 255.255.255.0 |<br /> | 192.168.0.0 | 255.255.255.128 |<br /> +-------------+-----------------+<br /> 2 rows in set (0.00 sec)</tt>
>       </p>
>     </blockquote>
>     
>     <p>
>       Also, here are some functions for basically doing everything you can in the <tt>ipcalc</tt> utility. Yes, you probably should do these calculations in your native programming language, but these might be handy somewhere:
>     </p>
>     
>     <blockquote>
>       <p>
>         <tt>mysql> -- network address for a given ip and netmask (192.168.0.5/255.255.255.128 here):<br /> mysql> select inet_ntoa(inet_aton('192.168.0.5') & inet_aton('255.255.255.128')) as network;<br /> +-------------+<br /> | network |<br /> +-------------+<br /> | 192.168.0.0 |<br /> +-------------+<br /> 1 row in set (0.00 sec)</p> 
>         
>         <p>
>           mysql> -- broadcast address for a given ip and netmask (192.168.0.5/255.255.255.128 here):<br /> mysql> select inet_ntoa(inet_aton('192.168.0.5') | (inet_aton('255.255.255.128') ^ (power(2, 32) - 1))) as broadcast;<br /> +---------------+<br /> | broadcast |<br /> +---------------+<br /> | 192.168.0.127 |<br /> +---------------+<br /> 1 row in set (0.00 sec)
>         </p>
>         
>         <p>
>           mysql> -- first "usable" ip for a given ip and netmask (192.168.0.5/255.255.255.128 here):<br /> mysql> select inet_ntoa((inet_aton('192.168.0.5') & inet_aton('255.255.255.128')) + 1) as first_usable;<br /> +--------------+<br /> | first_usable |<br /> +--------------+<br /> | 192.168.0.1 |<br /> +--------------+<br /> 1 row in set (0.00 sec)
>         </p>
>         
>         <p>
>           mysql> -- last "usable" ip for a given ip and netmask (192.168.0.5/255.255.255.128 here):<br /> mysql> select inet_ntoa((inet_aton('192.168.0.5') | (inet_aton('255.255.255.128') ^ (power(2, 32) - 1))) - 1) as last_usable;<br /> +---------------+<br /> | last_usable |<br /> +---------------+<br /> | 192.168.0.126 |<br /> +---------------+<br /> 1 row in set (0.00 sec)
>         </p>
>         
>         <p>
>           mysql> -- number of "usable" ips for a given netmask (255.255.255.128 here):<br /> mysql> select inet_aton('255.255.255.128') ^ (power(2, 32) - 2) as num_usable;<br /> +------------+<br /> | num_usable |<br /> +------------+<br /> | 126 |<br /> +------------+<br /> 1 row in set (0.00 sec)
>         </p>
>         
>         <p>
>           mysql> -- wildcard for a given netmask (255.255.255.128 here):<br /> mysql> select inet_ntoa(inet_aton('255.255.255.128') ^ (power(2, 32) - 1)) as wildcard;<br /> +-----------+<br /> | wildcard |<br /> +-----------+<br /> | 0.0.0.127 |<br /> +-----------+<br /> 1 row in set (0.00 sec)
>         </p>
>         
>         <p>
>           mysql> -- netmask for a given cidr (/25 here):<br /> select inet_ntoa(power(2, 32) - power(2, (32 - 25))) as netmask;<br /> +-----------------+<br /> | netmask |<br /> +-----------------+<br /> | 255.255.255.128 |<br /> +-----------------+<br /> 1 row in set (0.00 sec)
>         </p>
>         
>         <p>
>           mysql> -- cidr for a given netmask (255.255.255.128 here):<br /> select 32 - bit_count(power(2, 32) - inet_aton('255.255.255.128') - 1) as cidr;<br /> +------+<br /> | cidr |<br /> +------+<br /> | 25 |<br /> +------+<br /> 1 row in set (0.00 sec)</tt>
>         </p></blockquote>
