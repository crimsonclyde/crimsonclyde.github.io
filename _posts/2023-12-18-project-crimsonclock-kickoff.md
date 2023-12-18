---
layout: post
title: "I♥Craft: CrimsonClock - Wordclock Kickoff"
categories: I♥Craft
---

## Origin

The fascinating origins of the modern word clock can be traced back to Germany, thanks to the innovative creation known as QlockTwo. This unique timepiece was born from the playful ingenuity of two friends. But what exactly is a word clock? Unlike traditional clocks, a word clock displays time in a textual format, illuminating phrases like "It's a quarter to twelve" or "It is five minutes before six" amidst a grid brimming with letters.

For over a decade, I've been captivated by this ingenious design. Regrettably, QlockTwo clocks come with a steep price tag, but for those with a DIY mindset, creating your own version is within reach. With a microcontroller, some electronic components, and a healthy dose of creativity, you can embark on this project. The internet abounds with such DIY endeavors, and I've selected a particularly intriguing one from[TechnicController.com](https://techniccontroller.com).

## Default Matrix (Cardboard)

![Cardboard Seperator Matrix by TechnicController.com](/assets/pix/CrimsonClock_CardboardMatrix.png)
*Source: [TechnicController.com](https://techniccontroller.com)

The standard matrix of this project utilizes cardboard, finished with a mirror coating. This choice is practical since the material remains unseen in the final assembly. Admittedly, I'm not the most skilled when it comes to handling scissors - straight cuts seem to be my nemesis, perhaps a challenging task for my limited brainpower which I jokingly liken to 'Rocket Scissor Science'. However, I have a secret weapon: my moderate proficiency in 3D printing, despite my somewhat limited CAD skills.

So, here's to taking on this challenge with gusto. Let's raise a glass and say, "Hold my beer" - it's time to create something extraordinary.

## Version Alpha

### Math (Alpa)

We need some math to get the missing measurements from the cardboard seperator matrix.

|Details |Amount |Calculation| Result |
|--- |--- |---|---| 
|Chamber Walls| 11 | 11 x 33,25mm | 365,75mm |
|Cut Outs     | 12 | 12 x 0,5mm   |   6,00mm |
|Total        |    |              | 371,75mm |

Now we substract from the total lenght the chamber walls and the cut outs so we have to total excess ends as leftovers and devide them by two.

|Details |Amount |Calculation| Result |
|--- |--- |---|---| 
|Total Excess Ends |   | 400 - 371,75mm  |  28,250mm  |
|Excess Ends       | 2 | 28,25 / 2       |  14,125mm  |

### CAD (Alpha)

Ok perfect now into the CAD program and create the sketch.

![CAD alpha part](/assets/pix/CrimsonClock_Sketch_CardboardMatrix.png)

Extrude to 4.6mm and fire up the printer.

**Pros:**

- Print fast
- Nearly to no filament
- Fits
  
**Cons:**

- Flimsy like hell
- Assembly really tricky

Bummer! Back to the drawing board clyde.

![Printed alpha part](/assets/pix/CrimsonClock_Cardboard_Matrix_Printed.JPG)

## Version Beta

We need to improve the strength, means more extrusion on the part should do the trick. Again we do the math. We reduce the chamber walls and the excess ends on each side by 0.25mm this should give us on each cutout double the sice.

### Math (Beta)

|Details |Amount |Calculation| Result |
|--- |--- |---|---|
|Chamber Walls| 11 | 11 x 32,75mm | 360,25mm |
|Cut Outs     | 12 | 12 x 10,00mm |  12,00mm |
|Total        |    |              | 372,25mm |

Now we substract from the total lenght the chamber walls and the cut outs so we have to total excess ends as leftovers and devide them by two.

|Details |Amount |Calculation| Result |
|--- |--- |---|---|
|Total Excess Ends |   | 400 - 372,25mm  |  27,750mm  |
|Excess Ends       | 2 | 27,75 / 2       |  13,875mm  |

### CAD (Beta)

Ok down to business and some more improvements. We need to avoid light bleeding, and to slam the matrix flat on the surface we add a few cutouts for the LED light strip. Should improve things. Really in my mind this is a brilliant idea.
And while we are in CAD, here the trick to get 400mm on the brintbead. Cut the piece in the middle and add a dovetail. But to clearify this - with such thin parts, it's mostly pointless but nice to have. So why not.

![CAD beta part](/assets/pix/CrimsonClock_CAD_beta.png)

Same procedure as before - extrude and sent of for printing.
But you already guess it, right, it will be three times the charm, inshallah!

![CAD beta part](/assets/pix/CrimsonClock_Printed-Beta.JPG)

**Pros:**

- Print fast
- Fits together nicely
- More stable
  
**Cons:**

- Still flimsy
- Assembly still tricky

## Version Final

### Math Final
No, not this time, we have already everything and this is just simply a different approach to get a grid done.

### CAD Final

Drop the inital idea to just recreate the cardboard origin. Why not just print the entire matrix in 4 parts? I think we should give it a try, right?

![CAD part final](/assets/pix/CrimsonClock_CAD_final.png)

Looks more complicated than it really is. Repeats itself over and over again. Between each square and the outside we have double walls, which we extrude in the end and voilá, our matrix without a lot of assembly.

![Printed part final](/assets/pix/CrimsonClock_Printed-Final1.JPG)
![Printed part final clockface](/assets/pix/CrimsonClock_Printed-Final2.JPG)

**Pros:**

- Perfect fitting
- Rock solid
- Aseembly easy just glue 4 parts together
  
**Cons:**

- Slow printing (around 4h each part in total 16h)
- More material

This is what we want. Printing still in progress.