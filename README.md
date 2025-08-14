# SEAL

[![License: CC BY-SA 4.0](https://img.shields.io/badge/License-CC_BY--SA_4.0-lightgrey.svg)](https://creativecommons.org/licenses/by-sa/4.0/)
[![Version](https://img.shields.io/badge/Version-1.0-blue.svg)](https://github.com/AnmiTaliDev/seal)

SEAL is a secure binary format designed for storing encrypted personal data with password-protected field access. Perfect for storing user profiles, authentication data, and sensitive personal information.

## 🔐 Features

- **Password-Protected Encryption**: Sensitive fields are encrypted with AES-256-CBC
- **Flexible Data Storage**: Support for names, emails, phone numbers, birthdays, UUIDs, and metadata
- **Industry-Standard Security**: PBKDF2 key derivation with 100,000 iterations
- **Binary Efficiency**: Compact binary format with indexed sections for fast access
- **Cross-Platform**: Platform-independent binary format
- **Extensible**: Easy to add new field types and features

## 📋 Supported Fields

| Field | Description | Encrypted |
|-------|-------------|-----------|
| Name | First name | ❌ |
| Surname | Last name | ❌ |
| Password | bcrypt hash | ❌ |
| Birthday | Date of birth | ✅ |
| Email | Email address | ✅ |
| Phone | Phone number with country code | ✅ |
| UUID | Unique identifier | ❌ |
| Metadata | Additional information | ❌ |

## 🏗️ File Structure

```
┌─────────────────┐
│ Magic Number    │ 1F 53 45 41 4C 1A 0A 00
├─────────────────┤
│ Header          │ Version, flags, section count
├─────────────────┤
│ Sections        │ Data sections (some encrypted)
├─────────────────┤
│ Section Index   │ Quick access index
├─────────────────┤
│ Footer          │ File validation
└─────────────────┘
```

## 📖 Documentation

- **[English Specification](SEAL_FORMAT_SPEC.md)** - Complete technical specification
- **[Русская спецификация](SEAL_FORMAT_SPEC-RU.md)** - Полная техническая спецификация на русском

## 🔧 Magic Number Breakdown

The SEAL magic number `1F 53 45 41 4C 1A 0A 00` consists of:

- `1F` - Unique prefix marker
- `53 45 41 4C` - ASCII "SEAL"
- `1A` - Control byte (SUB character)
- `0A` - Newline character
- `00` - Null terminator

## 🛡️ Security Features

### Encryption
- **Algorithm**: AES-256-CBC
- **Key Derivation**: PBKDF2-HMAC-SHA256
- **Iterations**: 100,000
- **Salt**: 16 random bytes per file
- **IV**: 16 random bytes per encrypted section

### Password Storage
- Passwords are stored as bcrypt hashes with salt rounds = 12
- Never stored in plaintext

### Data Integrity
- CRC32 checksums for each section
- File size validation in footer
- Magic number validation

## 📁 File Extension

SEAL files use the `.seal` extension and MIME type `application/x-seal`.

## 🎯 Use Cases

- **User Authentication Systems**: Store user profiles with encrypted sensitive data
- **Personal Data Management**: Secure storage of personal information
- **Identity Documents**: Digital storage of ID information
- **Contact Management**: Encrypted contact databases
- **Backup Systems**: Secure personal data backups

## 🔄 Version History

- **v1.0** (Current): Initial specification with basic encryption and standard fields

## 🤝 Contributing

We welcome contributions! Please read our contributing guidelines and:

1. Fork the repository
2. Create a feature branch
3. Implement your changes
4. Add tests and documentation
5. Submit a pull request

## 📄 License

This project is licensed under the Creative Commons Attribution-ShareAlike 4.0 International License.

You are free to:
- **Share** — copy and redistribute the material in any medium or format
- **Adapt** — remix, transform, and build upon the material

Under the following terms:
- **Attribution** — You must give appropriate credit to AnmiTaliDev
- **ShareAlike** — If you remix, transform, or build upon the material, you must distribute your contributions under the same license

See the [LICENSE](https://creativecommons.org/licenses/by-sa/4.0/) file for details.

## 👨‍💻 Author

**AnmiTaliDev**
- GitHub: [@AnmiTaliDev](https://github.com/AnmiTaliDev)
- GitLab: [@AnmiTaliDev](https://gitlab.com/AnmiTaliDev)

## 🔗 Links

- **GitHub Repository**: https://github.com/AnmiTaliDev/seal
- **License**: [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/)

---

*SEAL format is designed for defensive security purposes. Always follow security best practices when implementing and using SEAL files.*