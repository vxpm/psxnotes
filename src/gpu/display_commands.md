# GP1 - Display Commands
> [!NOTE]
> This page is mostly intended as encoding reference - for more information about behaviour consult
> [psx-spx](../references.md#psx-spx).

Commands have an opcode defined in bits 24..30 of the first packet:
| Opcode        | Name                      | Alias             |
| ------------- | ------------------------- | ----------------- |
| `0x00`        | Reset GPU                 | GP1(00)           |
| `0x01`        | Reset Command Buffer      | GP1(01)           |
| `0x02`        | Acknowledge GPU Interrupt | GP1(02)           |
| `0x03`        | Display Enable/Disable    | GP1(03)           |
| `0x04`        | DMA Direction             | GP1(04)           |
| `0x05`        | Display Area Start        | GP1(05)           |
| `0x06`        | Horizontal Display Range  | GP1(06)           |
| `0x07`        | Vertical Display Range    | GP1(07)           |
| `0x08`        | Display Mode              | GP1(08)           |
| `0x09`        | Set VRAM Size (v2)        | GP1(09)           |
| `0x0A..=0x0F` | Unknown                   | GP1(0A)..=GP1(0F) |
| `0x10`        | Read GPU Register         | GP1(10)           |
| `0x11..=0x1F` | Mirrors of GP1(10)        | GP1(11)..=GP1(1F) |
| `0x20`        | Set VRAM Size (v1)        | GP1(11)..=GP1(1F) |
| `0x21..=0x3F` | Unknown                   | GP1(21)..=GP1(3F) |

None of the display commands require extra packets.
