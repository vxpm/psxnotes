# GP0 - Rendering Commands
> [!NOTE]
> This page is mostly intended as encoding reference - for more information about behaviour consult
> [psx-spx](../references.md#psx-spx).

Commands have an opcode defined in bits 29..32 of the first packet:
| Opcode | Name              | Alias   |
| ------ | ----------------- | ------- |
| `0x0`   | Misc              |         |
| `0x1`   | Polygon           |         |
| `0x2`   | Line              |         |
| `0x3`   | Rectangle         |         |
| `0x4`   | VRAM to VRAM blit |         |
| `0x5`   | CPU to VRAM blit  | GP0(A0) |
| `0x6`   | VRAM to CPU blit  | GP0(C0) |
| `0x7`   | Environment       |         |

## 0x00 - Misc
Misc commands have another opcode defined in bits 24..29 of the first packet:
| Misc Opcode | Name              | Alias   |
| ----------- | ----------------- | ------- |
| `0x00`        | NOP               | GP0(00) |
| `0x01`        | Clear Cache       | GP0(01) |
| `0x02`        | Quick Rect Fill   | GP0(02) |
| `0x03`        | Unknown*          | GP0(03) |
| `0x04..0x1E`  | NOP               |         |
| `0x1F`        | Interrupt Request | GP0(1F) |

<sub>\* Takes space in the FIFO, but seems like a NOP otherwise</sub>

## 0x07 - Environment
Environment commands have another opcode defined in bits 24..29 of the first packet:
| Environment Opcode | Name                          | Alias             |
| ------------------ | ----------------------------- | ----------------- |
| `0x00`             | NOP                           | GP0(E0)           |
| `0x01`             | Drawing Settings              | GP0(E1)           |
| `0x02`             | Texture Settings              | GP0(E2)           |
| `0x03`             | Set Drawing Area Top-Left     | GP0(E3)           |
| `0x04`             | Set Drawing Area Bottom-Right | GP0(E4)           |
| `0x05`             | Set Drawing Offset            | GP0(E5)           |
| `0x06`             | Mask Settings                 | GP0(E6)           |
| `0x07..=0x1F`      | NOP                           | GP0(E7)..=GP0(FF) |
