---
layout: post
title: "I♥Craft: LL Lantern - Electronics Investigation II"
categories: I♥Craft
---

##### Related posts  

[Electronics Investigation](https://clyde.crimson.space/posts/20230620/)\
[A background saga](https://clyde.crimson.space/posts/20230614/)\
[Back tile finish](https://clyde.crimson.space/posts/20230615/)

## Problem description
Still the same problem from [Part I](https://clyde.crimson.space/posts/20230620/)

tl;dr: 
The LED matrix doesn't work as soon as I attach a extension lead between the power supply and the microcontroller (ESP32) to the LED matrix panels. Period.

**What happens?**  
All animations and light controls work fine as long as there is only a tiny 10 cm cable between them. But attaching a 2,3m extension, things went south.  
Animations stop working, colours are wrong and all sorts of other strange things.

## In search of the culprit


### Resistor

Three hours later, middle of the night. We found what we have searched for.  
But first and foremost: **A big shoutout to @teschi for his help and again his Osciloscope which saved the day once again!**.  
The resistor in my data line was located before the extension lead which drops to much current and with the extension in place the signal received by  
matrix was not sufficent to be in the specifications of the WS2812B Neopixel RGB LEDs.


***Proof***

Fig. 1:   
The yellow line shows the readout right after the resistor but before the extension lead.  
The green line shows the readout right before the first LED after the extension lead.

What do you see a spike and a drop before every 0 or 1 and a Volatae which is to low (not shown on the picture).  
The resistor basically acts as a filter if used before the extension makes the situation worse instead of improving it.

![Osciloscope readout 1](/assets/pix/LLL_Oszi_1.png) 

Fig 2:  
Shows mostly the same as on Fig. 1.  
This time though, the spikes are segnificantly smaler and the voltage is a tiny bit higher.   
This is already enough that the colors and animations start to work correctly.  

![Osciloscope readout 2](/assets/pix/LLL_Oszi_2.png) 


***Three hours rly?***

Well of course this was not the only one we tried so here's an add-on for those still with me:  

What we tried is to sacrifice two WS2812G LEDs to act as level shifter in combination with a diode.  
Doesn't properly work in our case, so we decided to just go for the solution to place the resistor to the   
LEDs instead of to the ESP32 microcontroller. 


![Level Shifter Replacement](/assets/pix/LLL_LevelShifterLED.JPG) 

A bit puzzled? Well, here some details, that hope sched some light onto it:

![Level Shifter Replacement](/assets/pix/LLL_LevelShifterExplained.JPG)  


Again stay calm, it's not to hard to understand.  
First and most important to know is: NeoPixels powered by 5V ideally require a 5V data signal.  
Well, we don't have this. We power the strip with 5V from our power supply but the signal from the ESP32 is 3.3V.
Which is with short cables and only a bunch of LEDs no problem at all.

But we have 128 LEDs and a 2.5 meter cable in between.  
Well they designed it to accept a range of power in percent. Means 5V but the data pin works still ok if you get >3.5V

So why do we sacrifice the LED with a diode?  
We can lower the strips mains voltage from 5V to let's say 4.5V which means =>3.1V are now totally fine because of the 
percentage difference.

Basic concept and way better description: 
[Ardafruit Über Guide](https://hackaday.com/2017/01/20/cheating-at-5v-ws2812-control-to-use-a-3-3v-data-line/)