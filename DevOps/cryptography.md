
# CRYPTOGRAPHY

By: Sumeet Chand

Date: October 2024

# TABLE OF CONTENTS
- [1. Terminologies](#terminologies)
- [2. Common Commands](#common-commands)


# TERMINOLOGIES

Checksums are a means of validating a digital file by examinig all the bytes/data of a file and using a CRC-32 a mathamatical formulae. There are different formulaes to use such as CRC-32, SHA1, or MD5 each with their own pros and cons listed below in order from best top to worse bottom. Newer methods use PGP signatures which include downloading keys alongside a file to verify it's integrity from the developer.

SHA-256: Part of the SHA-2 family, it is much more secure and is increasingly recommended for cryptographic security.

SHA-1: More secure than MD5, but still vulnerable to certain attacks; it's also used for checksumming but is being phased out for sensitive applications.

MD5: Originally designed for checksums, it’s now often considered insecure against deliberate alterations but is still 
used for checksums in many applications due to its speed.

CRC-32: Good for detecting simple errors, fast, but not as secure against intentional tampering.

PGP Signature ____________________________________________________________

SUMMARY
Best for security: SHA-256
Decent for general use: SHA-1
Fast but insecure: MD5
Good for basic error-checking: CRC-32

This website can be used for drag and dropping any digital file to find the checksum: https://emn178.github.io/online-tools/crc32_checksum.html 

## CRC-32

CRC-32 is a type of checksum which is a means of validating a digital file by examinig all the bytes/data of a file and using a CRC-32 a mathamatical formulae to come up with a value which defines a file. E.g, a ROM dump of "Tetris" for Gameboy will have a CRC-32 of the below depending on the region of the game;

* Tetris (World) (Rev 1) CRC-32: 46df91ad
* Tetris (Japan) (En) CRC-32: 63f9407d

So when the original game ROM file is altered/hacked e.g, for adding color to the game, or creating custom borders for Super Gameboy use, the CRC-32 will calculate a different value.

This creates a means of testing the originality of a file, and also track down variations of it.

This website https://emn178.github.io/online-tools/crc32_checksum.html can be used for drag and dropping any digital file such as "Tetris" for Gameboy and finding any checksum such as a CRC-32 value. E.g, "46df91ad". This number coresponds to the ROM the author had copied is the original (World) region version as per above.


# COMMON COMMANDS

FIND AND COMPARE SHA256 OR MD5 FILE HASH
1. Download the zip file
e.g, this 64-bit zip download for a Gameboy Advance emulator file listed here - https://visualboyadvance.org/download/vba-m/
has a listed MD5 Hash of 7D86CF059BFCDBDE40F586EFDB020099
2. 
```bash
Get-FileHash "visualboyadvance-m-Win-x86_64.zip" -Algorithm MD5 # Windows
Get-FileHash "visualboyadvance-m-Win-x86_64.zip" -Algorithm SHA256 # Windows
md5 visualboyadvance-m-Win-x86_64.zip # Unix*
shasum -a 1 visualboyadvance-m-Win-x86_64.zip # Unix*

```
3. Compare the output. In this case it's the same as on the HTML page verifying the checksum is legitimate.
```bash
PS C:\Users\Sumeet\Downloads> Get-FileHash .\visualboyadvance-m-Win-x86_64.zip -Algorithm MD5
Algorithm       Hash                                                                   Path
---------       ----                                                                   ----
MD5             7D86CF059BFCDBDE40F586EFDB020099                                       
PS C:\Users\Sumeet\Downloads> Get-FileHash .\visualboyadvance-m-Win-x86_64.zip -Algorithm SHA256

Algorithm       Hash                                                                   Path
---------       ----                                                                   ----
SHA256          F656535EA605144B9444AB3DDBD93E5E3F2C27ECCC1225363787E8AB6FC092FE       
PS C:\Users\Sumeet\Downloads>
```

VERIFY PGP CERTIFIATE
```bash
# FOR WINDOWS

# The below example will download a test application e.g, the open source torrent software qBittorrent and download a PGP verifying software on Windows to verify it's integrity

1. Download the latest Windows qBittorrent software executable + click the link Signature and click to download the PGP signature. If you dont want to install thsi test application you can follow along the steps below to download any file
Application: https://www.fosshub.com/qBittorrent.html#
Signature https://www.fosshub.com/qBittorrent.html?dwl=qbittorrent_5.0.4_x64_setup.exe.asc
2. Go here https://www.gpg4win.org/get-gpg4win.html and click choose $______ (Free) option to download file
3. In bash for any Linux distro, find the file, and run the below command to match the Hash to the websites posted Hash here: https://www.gpg4win.org/package-integrity.html
SHA256 checksums
765673854c1503602b09c97bfa6c72b534e2414185fb2f23a0ce19cf8cecd891  gpg4win-4.4.0.exe
dbdbc14b60cc7bff92ea4103484202ef6fcac068838676fdd08e3e464dcb4b10  gpg4win-4.4.0.tar.xz


```bash
shasum -a 1 gpg4win-4.4.0.exe # Unix*

Algorithm       Hash                                                                   Path                                                          
---------       ----                                                                   ----                                                          
SHA256          765673854C1503602B09C97BFA6C72B534E2414185FB2F23A0CE19CF8CECD891       C:\Users\sumeet\downloads\gpg4win-4.4.0.exe   
```

4. The SHA256 Hash matches what's posted on the website. The file is legitimate. Install it. Accept all defaults including to start and run Kleopatra.
5.
```



HASH PASSWORD
1. Create file HashPassword.js and add any password to hash
```javascript
const bcrypt = require('bcryptjs'); // FOR SHA256, change to whatever you want

const password = 'Password1!';
const saltRounds = 10;

bcrypt.hash(password, saltRounds, (err, hash) => {
  if (err) {
    console.error('Error hashing password:', err);
  } else {
    console.log('Hashed password:', hash);
  }
});
```
2. run the file with node and the output value is the hashed password that can be entered anywhere
that is encrupted with that encryption (i.e, a MySQL DB with SHA256)
```bash
sumeetChand@Sumeets-Air backend % node HashPassword.js
Hashed password: $2a$10$9JsnQAMsal.4iMhX.CnZXuy9aCILeipHjLt8MGf5h.JJ64KSg.uOy
```
