# SPX Mirror Specification
SPX mirrors can be either made with a pre-made cross-platform Node.js script or using another language conforming to this specification.
## Upload Handling
Package uploads made using a mirror need to be forwarded with a MVID, a MID, and a MPWD, all in one combined structure that is then encoded in Base64URL.
Combined structure:

| **Type** | **Value** |
| -------- | --------- |
| 4 bytes | Magic number (ASCII `SMPX`) |
| 4 bytes | CiseRC checksum |
| 16 bytes | MVID |
| 16 bytes | MID |
| 24 bytes | Encrypted MPWD with MEK through LCipher |

`POST $spkgurl/muf?$b64s, Body = Package File Contents (Raw Binary)`
Package files include dependency information. LCipher is described in another specification.
## Package Updates
A handler must be made for POST /PKGUPDATE UGSFGCS F
