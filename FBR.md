# FBR-APP Specification
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

A relocation entry is just an offset of the place that needs to be re-located. The entry point gets 1 argument: FbrCall function address.
## FbrCall numbers
An Intel assembly stub has been made for 32-bit x86 systems which also works on 64-bit x86-64 by assembling it for x86-64.
```asm
FbrStart:
pop eax
mov [FbrCallAddr], eax
mov [BackupEsp], esp
call FbrMain
FbrExitPoint:
mov esp, [BackupEsp]
ret
FbrCallAddr: dd 0
FbrExit:
pop eax
jmp FbrExitPoint
BackupEsp: dd 0
FbrSetPixel:
push dword 0
jmp CallFbrCall
FbrDrawLine:
push dword 1
jmp CallFbrCall
FbrDrawRectOutOnly:
push dword 2
jmp CallFbrCall
FbrDrawRectFillOnly:
push dword 3
jmp CallFbrCall
FbrDrawRectFillOut:
push dword 4
jmp CallFbrCall
FbrDrawText:
push dword 5
jmp CallFbrCall
FbrDrawEllipseOutOnly:
push dword 6
jmp CallFbrCall
FbrDrawEllipseFillOnly:
push dword 7
jmp CallFbrCall
FbrDrawEllipseFillOut:
push dword 8
jmp CallFbrCall
FbrGetFbInfo
push dword 9
CallFbrCall:
jmp [FbrCallAddr]
```

| **#** | **Function name** |
| -- | -- |
| 0 | FbrSetPixel |
| 1 | FbrDrawLine |
| 2 | FbrDrawRectOutOnly |
| 3 | FbrDrawRectFillOnly |
| 4 | FbrDrawRectFillOut |
| 5 | FbrDrawText |
| 6 | FbrDrawEllipseOutOnly |
| 7 | FbrDrawEllipseFillOnly |
| 8 | FbrDrawEllipseFillOut |
| 9 | FbrGetFbInfo |
