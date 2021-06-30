# Decoder for 7-segment display

## 7-segment indicator

The 7-segment display is one of the most popular and simplest displays of Arabic numerals and other symbols. Patented as early as 1910 by [Frank Wood], it is still in demand today.

[Frank Wood]: https://patents.google.com/patent/US974943

![7-segment](https://github.com/MrZloHex/Decoder/blob/master/images/7-segment-photo.jpg)

Since LEDs are most commonly used in such displays, and as we know they have a cathode and anode, consequently the displays themselves are divided into 2 subtypes: common cathode and common anode. The differences are minimal, but sometimes there are cases where there is one higher priority option.

The indicator is a set of LEDs with either anode or cathode merged together, depending on the type. As we can see, it is possible to control each segment individually.

<img align="left" width="350" height="310" src="https://github.com/MrZloHex/Decoder/blob/master/images/display-pinout.jpg">

Let's take a common cathode display as an example. Let's try to output some digits to the display. First we take __0__; in order to output we need to connect pins _7, 6, 4, 2, 1, 9_ (_a, b, c, d, e, f_ - respectively) to "__+__" power, and don't forget about resistors, so that LEDs don't burn out; if you use _5V_ power, then it will be enough to connect 330Ohm resistor from pin _3, 8_ to _GND_. And then you can see __0__ on the display. You can also output __1__; connect pins _6, 4_ (_b, c_) to the positive terminal of the power supply and you can see the 1 on the display. The other digits are output in the same way. 

</br>

Made in Tinkercad

</br>

![Zero-One](https://github.com/MrZloHex/Decoder/blob/master/images/zerp-and-one.png)

## Problems that occur

We have figured out how we can display different characters on the indicator, but let's imagine that we have some electronic device which should count numbers and for convenience we want to use an indicator, but here is the trouble, we have only 4 free I/0 lines left, or we deliberately want to optimize this so as not to write code on the microcontroller to represent the numbers on the screen or in a more convenient way for us. Recall, a simple connection requires 8 of these lines.

## Encoding

As our most familiar number system is decimal, we will use it and the digits from 0 to 9. If desired, it is also possible to continue with the 16-digit system. Since we are still building with 5V logic, it would be strange not to use binary notation. We have 10 digits, hence we need at least 4 bits to represent all possible variations. I suggest making a table for convenience.

| Decimal |   0  |   1  |   2  |   3  |   4  |   5  |   6  |   7  |   8  |   9  |
|:-------:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
| Binary  | 0000 | 0001 | 0010 | 0011 | 0100 | 0101 | 0110 | 0111 | 1000 | 1001 |

Let's assume we have a 4-bit bus with our device transmitting a signal in binary representation. Now we need to decode it and transmit it to the display.

## Decoding

Using binary notation assumes the use of Boolean logic. So it makes sense to use it. Therefore we start by making a truth table for the decoder.

<img align="left" width="578" height="325" src="https://github.com/MrZloHex/Decoder/blob/master/images/truth-table.png">

For now, we will use a simple bus of 4 wires, each of which will carry a signal. In our case, 5V is a logical one and GND is a logical zero. The first column in the table is just for clarity, which digit we want to display. Based on this table we need to create a device that at input values in the bus, would output 7 values for the indicator. For this, we will use operations on Boolean values: conjunction (and, &), disjunction (or, |), negation (not, ¬). So, let's start creating a chain of logic gates.

## Circuit for segment 'a'

The truth table shows that segment __a__ must always be on, except for digits _1_ and _4_. To simplify, we can make it so that the logical 1 is only in these 2 states and add a logical _NOT_ to the output. Then we will always have a 1, except for the 2 states. Here is the schematic:

![Circuit](https://github.com/MrZloHex/Decoder/blob/master/images/circuit-segment-a.png)

I have added an additional inverted bus for convenience and clarity.

The output should be a logic _0_ with inputs _0 0 0 1_ and _0 1 0 0_ (on lines 3, 2, 1, 0 respectively). So I took not inverted first line but all other inverted lines, and it turned out, that _0 0 0 1_ transforms to _1' 1` 1' 1_ ( ' is inverted signal), then all these signals are grouped in one common, which goes into the _NOT-OR_ gate, which at the end inverts logical _1_ into _0_, so that at these input parameters segment __a__ is not on. The same is done for the other case: _0 1 0 0_ is converted to _1' 1 1 1'_ and also goes to the same _NOT-OR_ valve as the first case.

The rest of the modules for the other indicator segments are made in a similar way. You should end up with something similar:

![Full scheme](https://github.com/MrZloHex/Decoder/master/images/full-scheme.jpg)


