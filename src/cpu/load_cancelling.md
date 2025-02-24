# Load Cancelling
Sequential loads to the same register exhibit "load cancelling": non-terminal (i.e. not the last 
in a sequence) loads are completely discarded and only the last load will take effect.

This behaviour is required for passing some of the `CPU MEM DLY` tests in 
[Amidog's CPU test](../references.md#psxtest_cpu).

> [!NOTE]
> "Load" here refers to any kind of value move into a register, not only memory loads. This is
> specially important for instructions like `jal`, `bltzal` and `bgezal`, which modify `ra` and also
> exhibit load cancelling.

## Example
Consider the following program:
```
lb t0, 0($a0) // load value at address in a0 to t0
lb t0, 0($a1) // load value at address in a1 to t0
nop
nop
```
Since both loads have `t0` as their target register, load cancelling will happen and the first load
will not be visible at _any_ point in time. That is, up until the first `nop`, the value of `t0`
remains unchanged. It is only at the second `nop` that the value of `t0` changes to that of the 
value loaded by the second load:
```
lb t0, 0($a0) // t0 unchanged
lb t0, 0($a1) // t0 unchanged
nop           // t0 unchanged
nop           // t0 changed to the value loaded by second instruction
```
Here's another example:
```
lb t0, 0($a0)        // t0 unchanged
addi t0, r0, 0x0001  // moves 0x0001 into t0
nop                  // t0 is still 0x0001 - the first load was cancelled
```
