# c64_Machine_code_by_hand_example

a demonstration of hand coded machine code on c64
(c)2021 by ir. Marc Dendooven (Eldendo)

The program will reverse the screen of a c64.

First of all I will write the demo in basic:

A character on screen is reversed by reversing (XOR) the leftmost bit of the character.

In machine code we have the instruction EOR, but there is no XOR in basic,
So I defined it in a function FN XO.

The screen is contained in memory from address 1024 to 2023, but we use the range 1024 to 2047 (this bytes are not used)


10 def fn xo(b)=(b+128) and 255

100 for x=1024 to 2047
110 poke x,fn xo(peek(x))
120 next x

We can translate this from basic, but for a beginners example I will transform it first to a simpeler form not using values larger then 8 bit...


10 def fn xo(b)=(b+128) and 255
100 for x=0 to 255
110 poke x+1024,fn xo(peek(x+1024))
120 poke x+1280,fn xo(peek(x+1280))
130 poke x+1536,fn xo(peek(x+1536))
140 poke x+1792,fn xo(peek(x+1792))
150 next x

This basic program is very easy to translate to 6502 assembly language (left column)
you only need the programmers reference guide to understand what is does.

Now we translate by hand (no assembler program) to hexadecimal (colomn 2) or decimal numbers (column 3). Use the reference guide to translate.


LDX #0          A200        162 0
LDA #128        A980        169 128
EOR 1024,x      5D0004      93 0 04
STA 1024,x      9D0004      157 0 04
LDA #128        A980        169 128
EOR 1280,x      5D0005      93 0 05
STA 1280,x      9D0005      157 0 05
LDA #128        A980        169 128
EOR 1536,x      5D0006      93 0 06
STA 1536,x      9D0006      157 0 06
LDA #128        A980        169 128
EOR 1792,x      5D0007      93 0 07
STA 1792,x      9D0007      157 0 07
INX             E8          232
BNE -35         D0DD        208 221
RTS             60          96


now we just poke in the decimal values at location 49152 (this is a 4K region which is not used)

10 for i=0 to 37
20 read b
30 poke 49152+i,b
40 next i
100 data 162,0,169,128,93,0,4,157,0,4,169,128,93,0,5,157,0,5
110 data 169,128,93,0,6,157,0,6,169,128,93,0,7,157,0,7,232,208,221,96

And now start the MC program

sys 49152
