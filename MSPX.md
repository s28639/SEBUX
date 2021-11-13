# SPX Mirror Specification
SPX mirrors can be either made with a pre-made cross-platform Node.js script or using another language conforming to this specification.
## Upload Handling
Package uploads made using a mirror need to be handled by first fetching an MEK using a Base64URL, then forwarding the request with a MVID, a MID, and a MPWD, all in one combined structure that is then encoded in Base64URL.
Combined structure:

| **Type** | **Value** |
| -------- | --------- |
| 4 bytes | Magic number (ASCII `SMPX`) |
| 4 bytes | CiseRC checksum |
| 16 bytes | MVID |
| 16 bytes | MID |
| 24 bytes | Encrypted MPWD with MEK through LCipher |

```
GET https://sebux.herokuapp.com/mux?$b64MID, Response Body = MEK (Raw Binary)
POST https://sebux.herokuapp.com/muf?$b64s, Body = Package File Contents (Raw Binary)
```
Package files include dependency information because SEBUX is made to be light. LCipher is described in another specification.
## Package Updates and New Packages
A handler must be made for `POST /pu` that sends a `GET https://sebux.herokuapp.com/package/download?$pkgname` for each package listed in the `POST` request body (LF newline separated) that the mirror doesn't already have.
## Mirror Termination
If any request that involves an MID gives off a 555 error code in the response, this means that the mirror has been terminated. The only reason for this is if the mirror is owned and/or operated by bad guys.
### Handling
If there is a 555 error code, that mirror should set a termination flag so it does not make real MID requests but rather this request: `GET https://sebux.herokuapp.com/mtc?$b64MID`. If the response header `MT` is ASCII `0`, that same mirror can clear the termination flag.
## Download Handling
A request handler must be made for `GET /pkg/*.sbx` where the `*` is the package name.
Package names must start with a letter, end with a letter or digit, and the middle must be made of dashes, letters, and digits. Examples:

| ----- | ---- |
| Valid | lxde |
| Valid | build-essential |
| Invalid | 7zip |

The body must contain the file contents if the package exists with code 200 and application/octet-stream or empty body with code 404 and no MIME type if it doesn't exist.
## Root Directory Handling
A request handler for `GET /` must be made. Redirect it to https://sebux.herokuapp.com/?use_mirror=$b64MID.
