# Unaligned word loads and stores
These instructions are surprisingly confusing, but become easier to grasp once you understand their 
use case, which is performing unaligned reads/writes from/to memory.

## LWL
### Description
LWL's goals is reading the "left" (i.e. higher order) part of an unaligned word in memory into the 
"left" part of a register.

After computing the address `addr` to operate on, LWL performs the following:
- Iterate through the bytes of `rt`, from highest order to lowest (i.e. big endian)
- At the same time, also iterate through bytes in memory starting at `addr` and decreasing (since 
  the R3000 is little endian, this is also highest order to lowest)
- In each iteration, set the byte of `rt` to the byte in memory
- Stop once the byte address crosses a word boundary (in practice, this means you'll iterate exactly
  `addr % 4` times)

### Examples
```
// starting state
memory:        [0x00, 0x11, 0x22, 0x33, 0x44, 0x55, 0x66, 0x77, 0x88, 0x99, ..]
> address:     [0,    1,    2,    3,    4,    5,    6,    7,    8,    9,    ..]
> word_index:  [0,    0,    0,    0,    1,    1,    1,    1,    2,    2,    ..]

rt:            0xDEAD_BEEF

// operation: load left from address 6
lwl rt, 6($0)

// finishing state
memory:        [0x00, 0x11, 0x22, 0x33, 0x44, 0x55, 0x66, 0x77, 0x88, 0x99, ..]
> address:     [0,    1,    2,    3,    4,    5,    6,    7,    8,    9,    ..]
> word_index:  [0,    0,    0,    0,    1,    1,    1,    1,    2,    2,    ..]
rt:            0x6655_44EF
```
```
// starting state
memory:        [0x00, 0x11, 0x22, 0x33, 0x44, 0x55, 0x66, 0x77, 0x88, 0x99, ..]
> address:     [0,    1,    2,    3,    4,    5,    6,    7,    8,    9,    ..]
> word_index:  [0,    0,    0,    0,    1,    1,    1,    1,    2,    2,    ..]

rt:            0xDEAD_BEEF

// operation: load left from address 4
lwl rt, 4($0)

// finishing state
memory:        [0x00, 0x11, 0x22, 0x33, 0x44, 0x55, 0x66, 0x77, 0x88, 0x99, ..]
> address:     [0,    1,    2,    3,    4,    5,    6,    7,    8,    9,    ..]
> word_index:  [0,    0,    0,    0,    1,    1,    1,    1,    2,    2,    ..]

rt:            0x44AD_BEEF
```

## LWR
### Description
LWR's goals is reading the "right" (i.e. lower order) part of an unaligned word in memory into the 
"right" part of a register.

After computing the address `addr` to operate on, LWR performs the following:
- Iterate through the bytes of `rt`, from lowest order to highest (i.e. little endian)
- At the same time, also iterate through bytes in memory starting at `addr` and increasing (since 
  the R3000 is little endian, this is also lowest order to highest)
- In each iteration, set the byte of `rt` to the byte in memory
- Stop once the byte address crosses a word boundary (in practice, this means you'll iterate exactly
  `4 - addr % 4` times)

### Examples
```
// starting state
memory:        [0x00, 0x11, 0x22, 0x33, 0x44, 0x55, 0x66, 0x77, 0x88, 0x99, ..]
> address:     [0,    1,    2,    3,    4,    5,    6,    7,    8,    9,    ..]
> word_index:  [0,    0,    0,    0,    1,    1,    1,    1,    2,    2,    ..]

rt:            0xDEAD_BEEF

// operation: load right from address 3
lwr rt, 3($0)

// finishing state
memory:        [0x00, 0x11, 0x22, 0x33, 0x44, 0x55, 0x66, 0x77, 0x88, 0x99, ..]
> address:     [0,    1,    2,    3,    4,    5,    6,    7,    8,    9,    ..]
> word_index:  [0,    0,    0,    0,    1,    1,    1,    1,    2,    2,    ..]

rt:            0xDEAD_BE33
```
```
// starting state
memory:        [0x00, 0x11, 0x22, 0x33, 0x44, 0x55, 0x66, 0x77, 0x88, 0x99, ..]
> address:     [0,    1,    2,    3,    4,    5,    6,    7,    8,    9,    ..]
> word_index:  [0,    0,    0,    0,    1,    1,    1,    1,    2,    2,    ..]

rt:            0xDEAD_BEEF

// operation: load right from address 1
lwr rt, 1($0)

// finishing state
memory:        [0x00, 0x11, 0x22, 0x33, 0x44, 0x55, 0x66, 0x77, 0x88, 0x99, ..]
> address:     [0,    1,    2,    3,    4,    5,    6,    7,    8,    9,    ..]
> word_index:  [0,    0,    0,    0,    1,    1,    1,    1,    2,    2,    ..]

rt:            0xDE33_2211
```

## LWL and LWR example usage together
```
// starting state
memory:        [0x00, 0x11, 0x22, 0x33, 0x44, 0x55, 0x66, 0x77, 0x88, 0x99, ..]
> address:     [0,    1,    2,    3,    4,    5,    6,    7,    8,    9,    ..]
> word_index:  [0,    0,    0,    0,    1,    1,    1,    1,    2,    2,    ..]

rt:            0xDEAD_BEEF

// let's load the word at address 1
// operation: load right from address 1
lwr rt, 1($0)
// operation: load left from address 4
lwl rt, 4($0)

// finishing state
memory:        [0x00, 0x11, 0x22, 0x33, 0x44, 0x55, 0x66, 0x77, 0x88, 0x99, ..]
> address:     [0,    1,    2,    3,    4,    5,    6,    7,    8,    9,    ..]
> word_index:  [0,    0,    0,    0,    1,    1,    1,    1,    2,    2,    ..]

rt:            0x4433_2211
```

## SWL and SWR
These two instructions work the exact same as their load counterparts, except that instead of 
loading the bytes from memory and putting them into the bytes of `rt`, they store the bytes of `rt`
in memory.
