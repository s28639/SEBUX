# SPX - SEBUX Package File
SPX files include their dependency names in them for an enhanced package file. SPX files are made for SEBUX.
## Header
This header is 64 bytes. It is required to be at the beginning of the file.

| **Type** | **Value** |
| -------- | --------- |
| 4 bytes | Magic number (ASCII `SPKG`) |
| 4 bytes (`uint32_t`) | XSES checksum of following bytes |
| 4 bytes (`uint32_t`) | Dependency descriptor offset |
| 4 bytes (`uint32_t`) | Compressed file table and files offset |(DEFLATE) |
| 19 bytes | Reserved, set to 0 |
| 4 bytes | File offset of uninstall BASH script (ASCIZ format) |
| 16 bytes | Can be used for steganography or SPX extensions |
| 4 bytes (`uint32_t`) | Offset of 
| 1 byte (`uint8_t`) | Version |
| 4 bytes (`uint32_t`) | Decompressed size of file table and files

Version bitfield:

| 7:4 | 3:0 |
| :-: | :-: |
| Major | Minor |
## File table
The file table and files are compressed completely using DEFLATE. Each entry looks like this:

| **Type** | **Value** |
| -------- | --------- |
| 4 bytes | Magic number (ASCII `SPFE`) |
| 4 bytes (`uint32_t`) | Flags |
| 4 bytes (`uint32_t`) | Offset to file contents / directory first entry
| 4 bytes (`uint32_t`) | Offset of next entry or 0 if last entry |
| 12 bytes | Reserved |
| 4 bytes (`uint32_t`) | Offset of Unicode null terminated package name |

Flags:

| 31:8 | 7 | 6 | 5:0 |
| :-: | :-: | :-: | :-: |
| Reserved | Directory | Hidden when installed | Reserved |
## Dependency descriptor
SPX files include dependency lists inside the package file instead of having a separate Packages list. For more details about the reason, refer to the SPX mirror documentation.
### Header
| **Type** | **Value** |
| -------- | --------- |
| 8 bytes | Magic number (ASCII `SPXDEPLS`) |
| 4 bytes | Reserved |
| 4 bytes | File offset of first entry |
### Entry
| **Type** | **Value** |
| -------- | --------- |
| 3 bytes | Magic number (ASCII `SDP`) |
| 1 byte (`uint8_t`) | Bitfield |
| 4 bytes (`uint32_t`) | File offset of next entry |
| 4 bytes | Reserved |
| Variable | Dependency name (ASCIZ) |

Bitfield:

| 7:2 | 1:0 |
| :-: | :-: |
| Reserved | Dependency type |

Dependency types:

| **ID bits** | **Type** |
| ----------- | -------- |
| 00 | Requirement |
| 01 | Recommendation |
| 10 | Suggestion |
| 11 | Enhancement |
