# Manual Errata
A list of corrections of the [R3000 manual](/references.md#r3000-manual).

## SLTIU
The manual describes `SLTIU` as putting the result into `rt`, but show pseudocode that put's the 
result into `rd`. The pseudocode is wrong - `rt` is the correct register.
