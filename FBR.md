# FBR-APP Specification
The main FBR-APP 
## Script
```bash
#!/bin/bash
##FBR##
if test -f /bin/fbr
if $UID == 0
/bin/fbr
fi
else
echo ERROR: FBR (Framebuffer Renderer) not found.
exit 1
fi
exit #FAP
```
## Header

| **Offset** | **Type** | **Value** |
| ---------- | -------- | --------- |
| 0x00 (0) | 4 bytes | Magic number (ASCII `FBR` + HEX `00`) |
| 0x04 (4) | 4 bytes (uint32_t) | Section header offset |
| 0x08 (8) | 8 bytes | Reserved, set to 0 |
| 0x10 (16) | 8 bytes | Can be used for extensions |
| 0x18 (24) | 4 bytes (uint32_t) | Flags |
| 0x1C (28) | 4 bytes | Checksum of preceding and following data |

### Flags

| 31 | 30 | 29:0 |
| -- | -- | -- |
| Bitness (`0` = 32, `1` = 64) | ARM flag (`0` = x86, `1` = ARM) | Reserved

## Section header

| **Offset** | **Type** | **Value** |
| ---------- | -------- | --------- |
| 0x00 (0) | 4 bytes | Magic number (ASCII `FSH` + HEX `00)` |
| 0x04 (4) | 4 bytes (uint32_t) | `.text` offset |
| 0x08 (8) | 4 bytes (uint32_t) | `.text` size |
| 0x0C (12) | 4 bytes (uint32_t) | `.data` offset or 0 if no `.data` |
| 0x10 (16) | 4 bytes (uint32_t) | `.data` size |
| 0x14 (20) | 4 bytes (uint32_t) | `.rodata` offset or 0 if no `.rodata` |
| 0x18 (24) | 4 bytes (uint32_t) | `.rodata` size |
| 0x1C (28) | 4 bytes (uint32_t) | Entry point offset |
| 0x20 (32) | 12 bytes | Reserved |
| 0x2C (44) | 4 bytes (uint32_t) | Relocation table offset |
| 0x30 (48) | 14 bytes | Extension area |
| 0x3E (6) | 2 bytes (uint16_t) | # of relocation entries |

A relocation entry is just an offset of the place that needs to be re-located.
