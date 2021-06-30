# Decoder for 7-segment display

## 7-segment indicator

The 7-segment display is one of the most popular and simplest displays of Arabic numerals and other symbols. Patented as early as 1910 by [Frank Wood], it is still in demand today.

[Frank Wood]: https://patents.google.com/patent/US974943

![7-segment](https://github.com/MrZloHex/Decoder/blob/master/images/7-segment-photo.jpg)

Since LEDs are most commonly used in such displays, and as we know they have a cathode and anode, consequently the displays themselves are divided into 2 subtypes: common cathode and common anode. The differences are minimal, but sometimes there are cases where there is one higher priority option.

The indicator is a set of LEDs with either anode or cathode merged together, depending on the type. As we can see, it is possible to control each segment individually.

<img align="left" width="500" height="500" src="https://github.com/MrZloHex/Decoder/blob/master/images/display-pinout.jpg">

Let's take a common cathode display as an example. Let's try to output some digits to the display. First we take __0__; in order to output we need to connect pins _7, 6, 4, 2, 1, 9_ (_a, b, c, d, e, f_ - respectively) to "__+__" power, and don't forget about resistors, so that LEDs don't burn out; if you use _5V_ power, then it will be enough to connect 330Ohm resistor from pin _3, 8_ to _GND_. And then you can see __0__ on the display. You can also output __1__; connect pins _6, 4_ (_b, c_) to the positive terminal of the power supply and you can see the 1 on the display. The other digits are output in the same way. 

![Zero-One](https://github.com/MrZloHex/Decoder/blob/master/images/zerp-and-one.png)

## Problems that occur

We have figured out how we can display different characters on the indicator, but let's imagine that we have some electronic device which should count numbers and for convenience we want to use an indicator, but here is the trouble, we have only 4 free I/0 lines left, or we deliberately want to optimize this so as not to write code on the microcontroller to represent the numbers on the screen or in a more convenient way for us. Recall, a simple connection requires 8 of these lines.
