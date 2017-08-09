
## Ws28 Lib - A compact lib for WS2812B and 2813B

![RGB cube and strip](/CubeStrip.jpg)

#### The WS28 lib is an include file written in C. This means one simple file to refer to, one setup function to call.
#### See __[RGB circuits][1]__ for a survey of chips. The main functions are explained below. It is easy of course to add functions.

### WS28 differences with Neopixel
NeoPixel see the LEDs as succesive variables in a structure. You can address randomly the structure and when satisfied, you ask for the transfer. Due to compatibility to different hardware and portability, the software is huge and requires a memory space proportional to the length of the strip
Smart pixel structure is indeed simple. A set of bytes is serially transferred and shown (copied on thre LED drivers) when a stop condition occurs (no transfer for 60µs in the case of WS2812/13).
With the , the rule is simple: One transmit bytes that are taken by the LEDs and when you stop transmitting for 60µs, the color is updated.
On a strip, every LED takes the first data and transmits consecutive datas. One do not need to know the length of the strip. Send data for 2 LEDS, only the first two will light, the next ones will keep theis state. Send Data for too many LEDs, additional data is ignored.

### WS2812/13 timing
WS2812/13 LEDs decode pulse length in order to recognize zeros and ones. Zeros are 370 to 490 nanoseconds, Ones are 600 to 11 00 nanoseconds. Space between pulses are up to the 60µs specified for an end of transmission, but this is not what is specified by the manufacturer. Critical timings, impossible to respect with a C program (accessing the next bit value takes too much time on a C-loop, one need to do bitbang in assembler). Bit transfer duration is adjusted with Nops. It looks reliable at 16 MHz on the AVR328, but not at 8MHz on the ATtiny24 and ATtiny25 family, where the first LED decodes correctly, but has strange transfer .

#### WS28 lib is based on two commands: __RGB (rr,gg,bb);__ which you repeat for the consecutive LEDs, and __Show();__ (which is a terminating delay of 100 us).

Use "for" loops, transfer tables, etc, and get all the color effects you wish, using less than one kilobyte of code.

### WS28 add also three useful primitives
__LogRGB(rr,gg,bb)__ because our eye has a logarithmic response to light intensity, and not a linear pwm response!

__Hue(hh)__ because we see colors as a spectrum and not as RGB leds

__LogHue(hh)__ for the two reasons above.

WS28 is indeed not a lib, but a set or inserted files you can use as you wish, written in easy to understand plain C you can compile on any C compiler and run on any microcontroller. You can remove what you do not use, and add your own functions.

__[Download]__(www.didel.com/WS28.zip) for examples and the include files. See http://www.didel.com/coursera/FichiersInclus.pdf if you are an Arduino fan not familiar with include files.

#### WS28 lib run with the ATtiny micros at 8MHz. See [WS28 for ATtinys][3]


[1]: http://www.didel.com/RGBstrips.pdf
[2]: http://www.didel.com/MiniCube.pdf
[3]: http://www.didel.com/Ws28ATtiny.pdf
