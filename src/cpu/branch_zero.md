# Encoding of BLEZ/BGEZ/BLEZAL/BGEZAL
Both [PSX-SPX](../references.md#PSX-SPX) and the [R3000 manual](../references.md#R3000-Manual) specify 
these instructions as having the following encoding:
```
000001 | rs   | 00000| <--immediate16bit--> | bltz
000001 | rs   | 00001| <--immediate16bit--> | bgez
000001 | rs   | 10000| <--immediate16bit--> | bltzal
000001 | rs   | 10001| <--immediate16bit--> | bgezal
```
This is only partially correct. The true encoding is the following:
```
000001 | rs   | xxxx0| <--immediate16bit--> | bltz
000001 | rs   | xxxx1| <--immediate16bit--> | bgez
000001 | rs   | 10000| <--immediate16bit--> | bltzal
000001 | rs   | 10001| <--immediate16bit--> | bgezal
```
Where `xxxx` is any bit sequence other than `1000`.

This behaviour is required for passing the `b_0xXX_y` tests in 
[Amidog's CPU test](../references.md#psxtest_cpu).
