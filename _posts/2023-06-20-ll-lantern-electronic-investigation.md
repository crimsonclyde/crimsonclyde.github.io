---
layout: post
title: "I♥Craft: LL Lantern - Electronics Investigation"
categories: I♥Craft
---

##### Related posts  
[A background sage](https://clyde.crimson.space/posts/20230614/)\
[Back tile finish](https://clyde.crimson.space/posts/20230615/)

## Problem description
So, where to start and how to explain properly, so you can follow along in the shortest possible ways?
Bear with me, we do it the easy way:

tl;dr: 
The LED matrix doesn't work as soon as I attach a extension lead between the power supply and the microcontroller (ESP32) to the LED matrix panels. Period.

**What happens?**
All animations and light controls work fine as long as there is only a tiny 10 cm cable between them. But attaching a 2,3m extension, things went south.  
Animations stop working, colours are wrong and all sorts of other strange things.

## In search of the culprit


### Voltage drop
I discussed this problem lately with Grisu and he mentioned a problem with some of his setup.\
Grisu needs to feed additional power to his LED strips in between, otherwise he expirerences color issues.

**Measuring Results**

| Extension | Measurment|
|---|---|
| NO | 5.2V|
| YES | 5.198V (+/- 0.001V)|


![Voltage Measurement](/assets/pix/LLL_Electronics_voltage.JPG)

I would assume, based on this result this is not the problem. 

### Capacitator

Marty, this is what makes time travel possible: the flux capacitor! Just jokin' but yes according to the [WLED documentation](https://kno.wled.ge/basics/getting-started/)  
a capacitor can hold a certain amount of charge for short terms of power consumption to get a fairly straight Voltage supply when we need it. At least in my understanding.  
I put the voltage regulator as close to the LED panels as possible and attached the probes of my multimeter.

Measurements is stable at 3.4V with a LEDs solid pattern, while with an animation running we have ~3.4 - ~4.6V. But still, the colors and the animation is just wrong.


![Capacitor](/assets/pix/LLL_Electronics_capacitor.JPG)

## Level/Shifter Translator

OK, so far so good, one thing left to test. A level shifter. Again we take another look to excelent WLED documentation:

> If the connecting wire cannot be kept short, use a level shifter/translator.

Hmm, that sounds promising, why not ask ChatGPT for help here?

>WS2812B LED strips are indeed sensitive to the voltage level on their data line, which should be close to the VCC voltage level of the LED strip. When the ESP32 and the LED strip are close together, 
>the voltage drop across the data line might be small enough that it doesn't cause a problem. But when you add an extension cable between the ESP32 and the LED strip, the voltage drop across the data
>line may increase to a point where it disrupts the communication between the ESP32 and the LED strip.
>
>A level shifter might indeed help here. A level shifter is a device that can change a digital signal from one voltage level to another. For your case, you want a level shifter that can shift the
>3.3V signal from the ESP32 to a 5V signal that can be used by the WS2812B LED strip.
>
>You might also consider using a logic level converter. This can shift the logic level of the data line from 3.3V (the output of the ESP32) to 5V (the logic level of the WS2812b).
>
>Some popular choices for this are the 74HCT125 or the 74AHCT125. These chips are quad level shifters, meaning they can shift up to four signals at once, although you'll likely only need one.


## Conclusion
We need a level shifter.  
I already bought some, but I need to further investigate them, no data sheet available.  
My guess right now

```
GND                              -->   GND
ESP GPIO D4 3.3V signal (GPIO)   -->   LV1
5V power                         -->   HV
Data PIN LED                     -->   HV1
GND                              -->   GND
```