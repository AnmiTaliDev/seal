# SEAL Binary Format Specification v1.0

## Overview

SEAL is a binary format designed for storing encrypted personal data with password-protected field access. Developed by AnmiTaliDev under CC BY-SA 4.0 license.

**Repositories:**
- GitHub: https://github.com/AnmiTaliDev/seal

## Magic Number

Every SEAL file begins with an 8-byte magic number:
```
1F 53 45 41 4C 1A 0A 00
```

**Breakdown:**
- `1F` - Unique prefix marker
- `53 45 41 4C` - ASCII "SEAL"
- `1A` - Control byte (SUB character)
- `0A` - Newline character
- `00` - Null terminator

## File Structure

```
[Magic Number: 8 bytes]
[Header: Variable]
[Sections: Variable]
[Footer: Variable]
```

## Header Structure

```
[Version: 1 byte]          // Format version (0x01)
[Flags: 1 byte]            // Feature flags
[Section Count: 2 bytes]   // Number of sections (little-endian)
[Reserved: 4 bytes]        // Reserved for future use
```

### Flags (8 bits)
- Bit 0: Encryption enabled
- Bit 1: Compression enabled
- Bit 2: Checksum present
- Bit 3: Extended metadata
- Bits 4-7: Reserved

## Section Structure

Each section follows this format:

```
[Section Type: 1 byte]     // Section identifier
[Flags: 1 byte]           // Section-specific flags
[Data Length: 4 bytes]    // Length of data (little-endian)
[Data: Variable]          // Section data
[Checksum: 4 bytes]       // CRC32 checksum (optional)
```

### Section Types

| Type | ID | Description |
|------|----|-----------|
| `NAME` | 0x01 | First name |
| `SURNAME` | 0x02 | Last name |
| `PASSWORD` | 0x03 | Hashed password |
| `BIRTHDAY` | 0x04 | Date of birth |
| `EMAIL` | 0x05 | Email address |
| `PHONE` | 0x06 | Phone number |
| `UUID` | 0x07 | Unique identifier |
| `METADATA` | 0x08 | Additional metadata |

### Section Flags (8 bits)
- Bit 0: Encrypted
- Bit 1: Compressed
- Bit 2: Required field
- Bit 3: UTF-8 encoded
- Bits 4-7: Reserved

## Data Types

### String Data
- Length-prefixed UTF-8 strings
- Format: `[Length: 2 bytes][UTF-8 Data: Variable]`

### Date Data (Birthday)
- 8 bytes: Year (2), Month (1), Day (1), Reserved (4)
- Format: Little-endian

### UUID Data
- 16 bytes: Standard UUID format
- Format: RFC 4122 compliant

### Phone Data
- Length-prefixed string with country code
- Format: `[Country Code: 2 bytes][Number Length: 2 bytes][Number: Variable]`

## Encryption

Encrypted sections use AES-256-CBC with PBKDF2 key derivation:

```
[Salt: 16 bytes]          // Random salt for PBKDF2
[IV: 16 bytes]            // Initialization vector
[Encrypted Data: Variable] // AES-256-CBC encrypted content
```

**Key Derivation:**
- Algorithm: PBKDF2-HMAC-SHA256
- Iterations: 100,000
- Salt: 16 random bytes
- Output: 32 bytes (256-bit key)

## Footer Structure

```
[Index Offset: 4 bytes]   // Offset to section index (little-endian)
[File Size: 4 bytes]      // Total file size (little-endian)
[Footer Magic: 4 bytes]   // Footer validation (0xDEADBEEF)
```

## Section Index

Located at the end of file (before footer), provides quick access:

```
[Section Count: 2 bytes]
For each section:
  [Type: 1 byte]
  [Offset: 4 bytes]
  [Length: 4 bytes]
```

## Example File Layout

```
Offset  | Content
--------|------------------
0x00    | Magic Number (8 bytes)
0x08    | Header (8 bytes)
0x10    | UUID Section
0x28    | NAME Section
0x45    | SURNAME Section
0x62    | PASSWORD Section
0x88    | EMAIL Section (encrypted)
0xB5    | PHONE Section (encrypted)
0xE2    | BIRTHDAY Section (encrypted)
0xFF    | Section Index
0x12A   | Footer (12 bytes)
```

## Security Considerations

1. **Password Storage**: Passwords are stored as bcrypt hashes, never in plaintext
2. **Encryption**: Uses industry-standard AES-256-CBC with proper key derivation
3. **Authentication**: Each encrypted section includes integrity validation
4. **Salt Uniqueness**: Each file uses unique salts for key derivation

## Implementation Notes

- All multi-byte integers are stored in little-endian format
- String encoding is UTF-8 unless otherwise specified
- CRC32 checksums use IEEE 802.3 polynomial
- File extension: `.seal`
- MIME type: `application/x-seal`