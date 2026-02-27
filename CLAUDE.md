# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a **hardware reverse engineering project** for the GoldenMate BMS (Battery Management System). There is no software build system, test suite, or linting -- the repository consists entirely of Obsidian-flavored markdown notes, images, and Excalidraw diagrams documenting the RE process.

## Target Hardware

- **MCU**: FM33LC026N (Fudan Microelectronics, ARM Cortex-M0 r0p0, ARMv6M, little-endian, Thumb mode)
- **BMS ASIC**: SH367309U
- **External EEPROM**: BL24C512A (I2C, connected to MCU pins PB3/SCL and PB4/SDA)
- **RS485 path**: MCU -> CA-IS3721HS (isolator) -> 3PEAK TP8485E -> RS485 bus
- **Power regulator**: AIPULNION FN1-05S05an, XLSEMI XL7015E1

## Debugging Setup

- **Debugger**: ST-LINK V2J37S7 over SWD (Serial Wire Debug)
- **Software**: OpenOCD (custom build with FM33LC026N support at `~/projects/openocd-code/`)
- **OpenOCD config**: `fm033026n_openocd/stlink.cfg` on the bench Linux machine
- **Target config**: `cortex_m` target type, `dapdirect_swd` transport, 100 kHz clock
- **SWD DPIDR**: 0x0bb11477, **CPUID**: 0x410cc300
- **GDB server**: port 3333, telnet on 4444, tcl on 6666

Stock OpenOCD (v0.12.0) fails with `Cortex-M PARTNO 0xc30 is unrecognized`; a patched build from source resolves this.

## Memory Map (known so far)

- Internal flash data observed from **0x0 to 0xf4e0**
- LDT1 OTP page at **0x1ffffc00** (value: `cc5533aa 0050ffaf ffffffff ffffffff`)
- IF0 OTP page region at **0x00040000** (reads all `0xffffffff`)
- RAM (MSP initialized to): **0x20001238**
- Reset vector / bootrom entry: firmware starts at the RESET vector; entry sled around **0x140**

## Key Investigation Threads

- Determining boot mode pin configuration and what memory is mapped at 0x0000
- Reading/dumping the external I2C EEPROM (BL24C512A) via CH341A programmer
- Analyzing firmware in Ghidra (interrupt handlers, thunk functions, boot sequence)
- Monitoring RS485 and UART4 signals with logic analyzer during boot
- Understanding internal flash type (NOR vs NAND) for proper OpenOCD read commands

## File Conventions

- Notes use **Obsidian markdown** with `![[wikilink]]` syntax for images and Excalidraw embeds
- Task tracking uses Obsidian checkbox format: `- [ ]` / `- [x]` with date stamps
- `PCB-layout-markup.jpg` is the annotated board photo referenced by `GoldenMate PCB Overview.md`
- `rs485-trace-mapping.excalidraw.md` contains the signal path diagram
