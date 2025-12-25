# Copilot Instructions for Vare Obfuscator 2.0

## Project Overview
- **Purpose:** Obfuscates Python code using AES encryption, junk code injection, and optional Windows executable signing to evade detection and protect intellectual property.
- **Main entrypoint:** `Vare.py` (CLI tool)
- **Key files:**
  - `Vare.py`: Main logic for obfuscation, junk code, and signing workflow
  - `stub`: Template for obfuscated output, with placeholders for injected code/data
  - `sigthief.py`: Handles Windows PE signature extraction/injection (used for `--sign`)
  - `sign.exe`: Example signed binary for signature stealing (Windows only)

## Usage & Workflows
- **Basic obfuscation:**
  ```sh
  python Vare.py <inputfile.py>
  ```
- **With junk code:**
  ```sh
  python Vare.py <inputfile.py> --junk
  ```
- **With signing (Windows):**
  ```sh
  python Vare.py <inputfile.py> --sign
  # Requires PyInstaller, sign.exe, and sigthief.py
  ```
- **Help:**
  ```sh
  python Vare.py --help
  ```

## Key Patterns & Conventions
- **Obfuscation:**
  - Uses AES (Fernet) to encrypt marshaled, compressed, base64-encoded Python bytecode.
  - Junk code is generated and injected via template placeholders (`%junk1%` ... `%junk5%`).
  - Output is written to `Obfuscated.py` (and optionally compiled to EXE).
- **Stub file:**
  - Contains placeholders for obfuscated data, keys, and junk code.
  - If using `--sign`, user must manually add all import dependencies to the top of `stub`.
- **Signature workflow:**
  - Uses `sigthief.py` to copy digital signatures from `sign.exe` to the generated EXE.
  - Only relevant for Windows targets.

## External Dependencies
- `cryptography` (Fernet)
- `PyInstaller` (for `--sign`)
- Windows PE signature tools (for `--sign`)

## Project-Specific Notes
- **Do not remove or rename placeholders in `stub`**; they are required for code injection.
- **All obfuscation logic is in `VareObfuscator` class in `Vare.py`.**
- **No test suite or CI/CD is present.**
- **No build system; all workflows are CLI-based.**

## Example: Adding a New Obfuscation Feature
- Add logic to `VareObfuscator` in `Vare.py`.
- Update `stub` if new placeholders or runtime logic are needed.
- Document new CLI options in `README.md`.

---
For questions, see the [README.md](../README.md) or contact the author via Telegram.
