---
title: "Product Idea: Raspberry Pi-based Music Server"
layout: post
tags: Product Raspberry-Pi
---

I often say to my friends that it would be really cool to start a company and make something. I've been toying with
the idea of some kind of device that lets you vote on what music to play next in the office or at parties. There are
currently other apps which do this, such as [music player daemon](http://www.musicpd.org), but they require a bunch of
configuration and to choose a front-end. What I would want is a box that you could just plug your 3.5 mm jack into, go
to a website, and start listening to and voting on music. As it just so happens, the Raspberry Pi model A+ is a cheap
computer that would be very suited to this task.
<!--more-->

I thought about the business plan for a while. Portable Bluetooth speakers have proven to be pretty popular, and there
are fewer devices that let you play audio via your phone over your home speaker system. I wanted a device that would
let an average person be able to plug a 3.5 mm jack into the box and have a remote music server. My box would be a
"smart" version of these boxes over WiFi: people would be able to upload MP3s and vote on them, and the device would
keep a library of recently played MP3s for people to vote on if they want to hear them again. I sat down with a pen
and paper and wrote a [very short business plan]({{site.baseurl}}/assets/2015-01-04-music-server.png): an elevator
pitch similar to the above, as well as the hardware needed (I'm not sure if a sound card is necessary), and the steps
to a product, from the very base product (a Raspberry pi with a web server and music playing daemon that lets you
upload songs and immediately plays them) to luxuries that I'd like to implement but most likely are not part of the
minimum viable product. These steps are as follows:

> 1. A Raspberry Pi running a web server and music playing daemon which allows people to upload songs and play them.
> 2. ... and vote on them by IP address.
> 3. ... and make accounts.
> 4. ... and easily find songs (search).
> 5. ... and the algorithm should weight votes unevenly so a user who rarely votes carries more weight.
> 6. ... and make it support "submitting" songs via AirPlay (long shot).

Items number five and six are obviously more technologically difficult to implement and less critical. While writing
this blog post however, I do think that having the device able to accept audio via AirPlay or Bluetooth, even if it is
not able to discern what counts as a song for voting purposes, would be an important feature for a minimum viable
product because it puts the box in the category of "bluetooth boxes you can hook up to your speaker system for
wireless music". These devices can be had
[on amazon for about $10](http://www.amazon.com/BestDealUSA-Bluetooth-Receiver-Adapter-Speaker/dp/B00ANDHBNS), so if
a roughly $50 smart box is going to compete with them, it needs to have a lot of value-add.

Using Bluetooth would necessitate also connecting a Bluetooth adapter to the Pi (and either the B+ or
a USB hub), and using AirPlay would lock out Android devices from streaming to the device. Both require a significant
amount of engineering effort put into implementing a way to consume AirPlay or Bluetooth streams. In terms of a
minimum viable product, it probably is not strictly necessary, and could likely be pushed out as a software update,
though Bluetooth would require that compatible hardware is built into the box.

I also feel as though a common problem home-networked devices have is discovery: people don't want to have to know the
IP address of a device on their local network to connect to it. I plan on solving this for my personal device by
giving it the DNS name [music.rchowe.com](http://music.rchowe.com) and configuring dynamic DNS on it. For a mass
market device, this is a little harder. Cisco routers use [routerlogin.net](http://www.routerlogin.net) which if you
are using your router's DNS server, gets intercepted and set to your router's IP address. Potentially I could write
some code to have the Pi try to grab an often unused IP address like `192.168.1.254` and get some domain name to
point to that, but it's risky that that IP address is already taken, or their network is using another form of
addressing (like mine, which uses `10.0.1.0/24`). A centrally hosted service combined with a unique number on the case
of each device seems like the best idea; basically dynamic DNS where each device keeps the central database updated
as to its local IP address and the central server redirects the browser to that local IP (possibly using some dynamic
DNS; so that if a user goes to `music.rchowe.com` they get redirected to `box2453.music.rchowe.com` which resolves to
their box's local address, but it's clumsy). Not an easy problem.

Ultimately, this sounds like (another!) good side project; I wouldn't put extraordinary expectations on it to succeed
in the marketplace or avoid being copied for cheaper, but if nothing else it would be really cool to build one for
myself and gauge people's interest about it.
