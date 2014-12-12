---
title: Plans for my DIY PCR Machine
layout: post
---

As a fun project with my Raspberry Pi, I decided to try to build a PCR machine.
<!--more-->
This is an attempt to get to know a little more about electrical engineering, and as a cool way to start doing biology at home. There already is an [instructibles post on how to build a PCR machine for under $85](http://www.instructables.com/id/Arduino-PCR-thermal-cycler-for-under-85/) using an Arduino mini, however I figured that since I already had a Pi, I'd use that and see if I could do it for slightly less. This is still a work in progress; up until now I've really just ordered parts, and haven't started putting anything together yet.

I am using the following materials to build my PCR machine:

* **Raspberry Pi Model B** ($35, Amazon). To avoid having to buy an LCD display, i'm going to make the machine internet controlled, effectively excluding the use of the model A. I'd like to use the model B+, but I already had a B.
* **Makerbot Aluminium Heater Block** ($5, Amazon). This is a heater block with pre-drilled 6 mm holes in it and a spot for a thermocouple. I expect to have to widen the hole very slightly to accomodate PCR tubes.
* **150 &Omega;, 50 W Wirewound Resistors** ($5 ea., Mouser). I use two of these resistors in series to heat the heating block.
* **5 V Raspberry Pi Cooling Fan** ($7, Amazon). I used this instead of a more common 12 V computer fan so that I could run it directly off of the Pi's 5V source.
* **5 V-controllable 10 A relay** ($7, Amazon). For turning power to the resistors off/on.
* **MCP3008-I/P Analog to Digital Converter** ($7, Amazon). This is for reading the thermocouple voltages using the Pi's GPIO. I could have bought a combined pre-made amplifier and A-D converter for about the same price, but I decided it would be more interesting to build my own.
* **Type K Thermocouple** ($12, Amazon). I'm not sure if the model I ordered is going to work because of the long probe; I'm investigating whether or not I can take the probe off.
* **Arctic Silver Thermal Paste** ($8, Amazon).

Additionally, I used jumper wire and a breadboard to prototype the low voltage components, and a power supply I already had to supply the high voltage components.

I also drew up some [circuit diagrams]({{site.baseurl}}/assets/PCR.pdf) of how I intend to put everything together.
