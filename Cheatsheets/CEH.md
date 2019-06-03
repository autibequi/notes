# CEH Cheatsheet

A mental dump while watching CEH courses and tuts.

[TOC]

## About

The notes in this file are based (but not limited) on  [The Complete Ethical Hacking Course](https://www.udemy.com/penetration-testing/).

## Environment

All commands here were executed on in the oficial `Kali Linux` docker image (because I’m too lazy to setup a full VM).

To create a container that will keep running in the background run:

> docker run -d --name kali kalilinux/kali-linux-docker tail -f /dev/null

If the container is not running simply execute:

> docker start kali

And to `ssh` over it run:

> docker exec -it kali bash

This setup will able the user to download more apps and keep it even in the case of a container restart.

The image comes barebones and you may need to install other applications. There is a list of [metapackages](https://www.kali.org/news/kali-linux-metapackages) that can be installed with bundle of softwares required to use these notes.

## Verbatum

* FUD - Fully Undetectable - ?
* RAT - Remote Access Tools
* VPS - Virtual Private Server - ?
* Reverse Shells - Opens a reverse connection to control current shell.

* 

## Tools

### The Onion Road

#### Links Trackers

- https://ahmia.fi/
- http://hiddenwiki.me/
- http://hiddenwikitor.org/
- https://thehiddenwiki.org/

#### Service

You can use tor as a proxy/vpn service simple install it.

> apt install tor

and start the process

> service tor start

### Proxychains

* Config File: 
  * /etc/proxychains.conf
* Configs
  * **dynamic_chain**: skip dead proxies (usefull if using free proxies)
  * **strict_chain**: doesn’t skip dead proxies
  * **random_chain**: randomly pick a proxy from list at each request
  * **proxy_dns**: avoid dns leaks that can lead to your location
* SOCKS5 is the best
* Add `socks5  127.0.0.1  9050` to the config file to add TOR as a proxy.

Proxychains call example:

> proxychains curl https://duckduckgo.com

