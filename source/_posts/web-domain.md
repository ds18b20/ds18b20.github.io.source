---
title: web-domain
date: 2018-11-04 18:44:54
categories: web
tags:
- web
- domain

---

# Register

网上有许多域名代理商，下面是目前在用的一个，

www.namesilo.com

# Manage

下面记录一下Domain绑定IP的设置方法：

> - **A Records**Using an A record, you can associate a hostname with an IPv4 address. For example, as of this writing, the A record for www.namesilo.com has 74.206.115.250 as its IPv4 address.
> - **AAAA Records**
>   Using an A record, you can associate a hostname with an IPv6 address. For example, as of this writing, the AAAA record for ipv6.l.google.com has 2001:4860:8011::63 as its IPv6 address.
> - **CNAME Records**
>   Using a CNAME record, you can associate a hostname with another hostname. This record type is commonly used when you need to associate a hostname with another service provider. For example, if you wanted to use Google Sites to host/manage your web site, you would create a CNAME record with "ghs.google.com" as the hostname value.
>   A common problem encountered with CNAME records when integrating with 3rd party site hosting services is that www.domain.com CNAME records are easily created, but creating the CNAME record for just domain.com (to capture traffic when people don't type in "www.") cannot be done since that goes against [RFC1034](http://tools.ietf.org/html/rfc1034). We have a work around for customers with this issue - our domain parking system redirects any "naked" domains (i.e. the SLD - domain.com) to the www.domain.com equivalent. Therefore, all that must be done is to create an A record to our parking IP (which can be done by applying the parking/forwarding DNS template) for the SLD hostname.
> - **MX Records**
>   Using an MX record (commonly referred to as a mail exchanger), you can associate a hostname which will be responsible for handling email processing. A MX record should be properly configured if you wish to receive email through a specific domain. MX records require a [distance value](http://en.wikipedia.org/wiki/MX_record#MX_preference.2C_distance.2C_and_priority) which controls delivery of mail; if no value is specified, it defaults to 10.
> - **TXT Records**
>   Using a TXT record, you can associate arbitrary text with a specific hostname. In practice, this is most commonly used to create [SPF records](http://en.wikipedia.org/wiki/Sender_Policy_Framework). If you need help creating a SPF record, you should first get familiar with SPF - you can also utilize this [SPF Wizard](http://www.microsoft.com/mscorp/safety/content/technologies/senderid/wizard/).
> - **SRV Records**
>   Using a SRV record allows you to associate the hostname and port number of servers for specified services. They are commonly used for VOIP/SIP applications. The hostname must begin with a format looking like _SERVICE._PROTOCOL. For example: _sip._tcp

最常用的是上面的A/AAAA/CNAME，其中Domain绑定IPv4时用A绑定；而绑定IPv6时使用AAAA绑定；绑定到另一个hostname时使用CNAME绑定。

# Reference

CNAME

https://xianhuo.org/namesilo-yumingjiexijiaocheng.html