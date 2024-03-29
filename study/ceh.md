# CEH Cheatsheet

A mental dump while watching CEH courses and tuts.

[TOC]

## About

The notes in this file are based (but not limited) on  [The Complete Ethical Hacking Course](https://www.udemy.com/penetration-testing/).

## Environment

The following setup will only works completly on Linux. The goddman osx runs docker inside a VM so it’s not possible to access the networks interfaces using it. If the command you want doesnt need direct access to the network interface (like wifi network attacks) then you may use it fine.

All commands here were executed on in the oficial `Kali Linux` docker image (because I’m too lazy to setup a full VM).

To create a container that will keep running in the background run:

> docker run --net="host" --privileged -d --name kali kalilinux/kali-linux-docker tail -f /dev/null 

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

* SSL Strip - Remove Pprotection from HTTPS requests

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

### proxychains

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

### openvpn

Usage:

> openvpn `A_FILE_FROM_WEBZ`.ovpn 

Make sure to exit all browsers before running it.

### macchanger

MAC address can leak data. 

XX:XX:XX:XX:XX:XX

The first three pairs identify the manufacture.

The last three can be set as anything by the manufacture.

The address doesnt leave the local network.

Usage

Revel current MAC address

> macchanger --show eth0

Randomize Adress

> macchanger -r eth0

Change Address

> macchanger --mac=`ADDRESS` eth0

### nmap

Network mapper

Available network to scan train

http://scanme.nmap.org/

Usage

> nmap -v -A scanme.nmap.org

### nslookup

Looks up for DNS information of an domain.

> nslookup scanme.nmap.org

### aircrack-ng

First change the wireless card to promíscuos Mode:

> ifconfig `CARDNAME` down
>
> ifconfig `CARDNAME` mode monitor
>
> ifconfig `CARDNAME` up

This will allow to the card read data not directed to it.

#### Bruteforce Cracking

> aircrack-ng -w `WORD-LIST-FILE` -e `ESSID`

* `ESSID` Target
* `WORD-LIST-FILE` File with words to be used in the attack.

### airmon-ng

Find processas that can cause trouble to aircrack.

> airmon-ng check `CARDNAME`

### airodump-ng

Scans for all networks available:

> airodump-ng `CARDNAME`

That will list informations like: BSSID, channel, signal strength, etc

Dumps da ta of a specific network:

> airodump-ng -c `CHANNEL` --bssid `BSSID` -w `RUNNAME`

### aireplay-ng

De-authenticate all devices of a defined network

> aireplay-ng --deauth `ITERATIONS` -a `BSSID`

* `ITERATIONS` Amount of deauth attack iterations. If equals `0` it will run as an infinite loop

#### DoS Attack

> aireplay --deauth 0 -a `BSSID` `NET INTERFACE`

### crunch

Creates a wordlist based on criteria you specify.

> crunch -t `PATTERN` -f `CHARSET-FILE`

The output can be piped to `aircrack-ng`.

* `PATTERN` Pattern of the words that will be created 

* `CHARSET-FILE` Charset of the keys used to build words.

### wash

Search for WIFI networks using a wificard.

> wash -i `interface`

Look for `WPS Locked` equals to `No`. 

Thoses networks can be cracked using [reaver](#reaver)

#### If something goes wrong run:

> mkdir /etc/reaver

### reaver

For cracking WPS protected networks.

WPS uses 4-8 numbers PIN.

It’s required to set the wifi card in promíscuos mode.

Run `arimon-ng check <<card>>` before to check for issues before running reaver.

> reaver -b `TARGET MAC ADDRESS` -i `INTERFACE` -c `TARGET CHANNEL` -r `TRIES`:`SECONDS`

* Look for reverse engineered scripts that can generate WPS PIN for the brand of the router that you are atacking. 
* Take care of AP Rate. If the router is blocked for WPS PIN you can try DoS the router so the owner will restart the router and you can try it again.

### sslstrip

>  

### dsniff

> 

## Miscellaneous

### Add interceptor to redirect requests from port 80 to 8080 

> iptables -t nat -A PREROUTING -ptcp --destination-port 80 -j REDIRECT --to-port 8080 

### ???

> echo 1 > /proc/sys/net/ipv4/ip_forward

### Manually Change Interface Channel

> iwconfig `INTERFACE` channel `NUMBER`

### Change local DNS provider

Usefull to avoid leak any data to untrustworthy DNS provider and/or to fix resolving issues.

Modify file 

>  vi /etc/dhcp/dhclient.conf

Restart network daemon

>service network-manager restart

Trusty and Secure DNS providers:

* [OpenDNS](https://www.opendns.com/setupguide/)

### Setup a CRON at reboot

Run:

> crontab -e

Add:

> @reboot /path/to/shell/script/file



### AWK

Usage:

Define `space` as a field delimiter and print the second column value.

> cat bleh.txt | awk -F " "  '{print $2}'

**Pro Tip**: Avoid piping data out of the expected format to AWK.



### ipinfo.io

Service to define an fisical location address of an IP.

> curl ipinfo.ip/`IP ADDRESS`

## Resources

* [Exploit Database](https://www.exploit-db.com/)