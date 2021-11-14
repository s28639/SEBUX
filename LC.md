# LCipher Specification
LCipher is used in the package-upload handling portion of SPX for verification over URL. It works on 24-byte data with a 24-byte key on 16 rounds.
## Encryption Round (keys go forward)
### Step 1: XOR Round Key
The 24-byte round key is XOR'ed into the data.
### Step 2: S-Box
For security, LCipher uses the same S-Box as AES. Run the forward S-Box on each byte of the data.
### Step 3: Mix
A mix is used:

| **Input** | **Output** | **Input** | **Output** | **Input** | **Output** | **Input** | **Output** |
| :-------: | :--------: | :-------: | :--------: | :-------: | :--------: | :-------: | :--------: |
| 0 | 5 | 1 | 6 | 2 | 4 | 3 | 8 |
| 4 | 7 | 5 | 11 | 6 | 2 | 7 | 23 |
| 8 | 3 | 9 | 15 | 10 | 12 | 11 | 14 |
| 12 | 21 | 13 | 18 | 14 | 16 | 15 | 17 |
| 16 | 1 | 17 | 0 | 18 | 13 | 19 | 22 |
| 20 | 9 | 21 | 19 | 22 | 10 | 23 | 20 |

## Decryption Round (keys go backward)
### Step 1: Unmix

| **Input** | **Output** | **Input** | **Output** | **Input** | **Output** | **Input** | **Output** |
| :-------: | :--------: | :-------: | :--------: | :-------: | :--------: | :-------: | :--------: |
| 0 | 17 | 1 | 16 | 2 | 6 | 3 | 8 |
| 4 | 2 | 5 | 0 | 6 | 1 | 7 | 4 |
| 8 | 3 | 9 | 20 | 10 | 22 | 11 | 5 |
| 12 | 10 | 13 | 18 | 14 | 11 | 15 | 9 |
| 16 | 14 | 17 | 15 | 18 | 13 | 19 | 21 |
| 20 | 23 | 21 | 12 | 22 | 19 | 23 | 7 |

### Step 2: Inverse S-Box
Run the inverse AES S-Box on each byte of the data.
### Step 3: XOR Round Key
The 24-byte round key is XOR'ed into the data.
## Key Schedule
### Key 1
Key 1 = Base Key ^ ce 09 75 c3 08 63 b1 a0 6b d9 7a d4 42 82 89 13 cf 74 a4 1a 71 c7 48 94
### Key 2
Key 2 = Base Key ^ 93 d9 38 eb f7 c8 bb 37 7c 5e d0 a9 97 1e 91 2d 76 90 05 db 07 41 e5 6d
### Key 3
Key 3 = Base Key ^ 16 53 f4 c3 31 75 6c 34 21 15 08 4d 2b 51 3d e2 88 1b 19 7a 1e c9 a6 e0
### Key 4
Key 4 = Base Key ^ 39 23 9a af c7 39 a8 4d 39 6f 5c ff 13 ac e0 95 cc 1c 3e 4f bc 18 43 36
### Key 5
Key 5 = Base Key ^ 5d 3d a1 e3 32 bd 7b 03 8f 0f 05 4e 20 64 21 8a b1 4a 49 b7 b1 0d 10 90
### Key 6
Key 6 = Base Key ^ 53 c0 56 19 d7 df 2d 37 28 f6 da f1 3a 24 14 ff 12 4e af 8a e0 bc e4 b0
### Key 7
Key 7 = Base Key ^ 19 fd ac 9a 5a ad 15 7b df fc d8 e0 9e 25 dc 2a 6c d8 b2 e2 e8 58 ed 73
### Key 8
Key 8 = Base Key ^ 9b 40 e1 ff cf 71 6f 34 5b f1 7c f7 9f cd a9 d2 62 8e e5 9a df 1f c6 f3
### Key 9
Key 9 = Base Key ^ eb 6c a2 13 72 46 0f e5 28 12 6a 2e d8 df 6d e0 47 52 20 03 c9 9b 60 29
### Key 10
Key 10 = Base Key ^ ad de 56 4a 8e 9a 3c a3 b6 98 36 23 f2 c3 1c d8 d8 63 1e 1d d5 02 9b e3
### Key 11
Key 11 = Base Key ^ dd 96 94 ff 30 6c 97 f0 01 54 31 d8 7c ba 74 70 1e 9a 17 f4 6d 74 37 62
### Key 12
Key 12 = Base Key ^ 48 96 cb 0b 32 a8 80 ed 1e 75 4d b7 5e ae 97 ba d1 66 e0 3c ae 37 9c ac
### Key 13
Key 13 = Base Key ^ fd 5a a4 0c 3a ed c2 28 48 1e 4b 65 40 df 25 8c ba dc 85 5d 75 b8 46 8a
### Key 14
Key 14 = Base Key ^ 78 cc 2f 36 1d bf c1 1c a8 94 87 ce 56 15 62 cb ac dd 33 bf 07 d4 cc 52
### Key 15
Key 15 = Base Key ^ 54 b7 92 25 ac 5a 11 35 78 cc 67 32 55 e7 0a a9 d3 53 ef 1f 7d 9a 7d d0
### Key 16
Key 16 = Base Key ^ b4 74 4e 69 3e ce d7 43 ba 5b 7c e1 39 17 c5 89 f5 4a 46 e0 d2 60 57 37
