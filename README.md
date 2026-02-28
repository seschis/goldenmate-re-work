# GoldenMate BMS Reverse Engineering

Hardware reverse engineering project for the GoldenMate BMS (Battery Management System). Documents PCB analysis, firmware extraction via SWD/OpenOCD, Ghidra reverse engineering work, and signal tracing.

## Target Hardware

| Component | Part | Notes |
|-----------|------|-------|
| MCU | FM33LC026N | Fudan Micro, ARM Cortex-M0 (ARMv6M), Thumb mode |
| BMS ASIC | SH367309U | |
| EEPROM | BL24C512A | I2C, MCU pins PB3/SCL and PB4/SDA |
| RS485 Isolator | CA-IS3721HS | |
| RS485 Transceiver | 3PEAK TP8485E | |
| Power | AIPULNION FN1-05S05an, XLSEMI XL7015E1 | |

## Debugging Setup

- **Debugger**: ST-LINK V2J37S7 over SWD (Serial Wire Debug)
- **Software**: OpenOCD (custom build with FM33LC026N support)
- **Transport**: `dapdirect_swd`, 100 kHz clock
- **SWD DPIDR**: `0x0bb11477` | **CPUID**: `0x410cc300`
- **GDB server**: port 3333, telnet 4444, tcl 6666

Stock OpenOCD v0.12.0 fails with `Cortex-M PARTNO 0xc30 is unrecognized`; a patched build from source resolves this.

## Memory Map

| Region | Address | Notes |
|--------|---------|-------|
| Internal flash | `0x0` - `0xf4e0` | Observed data range |
| LDT1 OTP | `0x1ffffc00` | `cc5533aa 0050ffaf ffffffff ffffffff` |
| IF0 OTP | `0x00040000` | Reads all `0xffffffff` |
| RAM (MSP init) | `0x20001238` | |
| Reset vector / entry | ~`0x140` | |

## Repository Structure

```
docs/       — Research notes from the RE process
images/     — Annotated PCB photos
diagrams/   — Signal path diagrams (Mermaid + Excalidraw)
datasheets/ — Component datasheets (including Chinese-to-English MCU translations)
```

- [docs/](docs/) — [Documentation index](docs/README.md)
- [images/](images/) — [Image index](images/README.md)
- [diagrams/](diagrams/) — [Diagram index](diagrams/README.md)
- [datasheets/](datasheets/) — [Datasheet index](datasheets/README.md)

## Key Documents

- [PCB Overview](docs/pcb-overview.md) — Component identification, pin tracing, task checklist
- [ST-LINK Debug Session](docs/stlink-debug-session.md) — OpenOCD/SWD debug session logs
- [Ghidra Analysis](docs/ghidra-analysis.md) — Firmware binary analysis (boot sequence, interrupt handlers)
- [Reading Flash](docs/reading-flash.md) — Flash/OTP memory dump observations
- [ARM Dev Tools](docs/arm-dev-tools.md) — ARM development and debugging tool links
- [RS485 Signal Path](diagrams/rs485-signal-path.md) — Mermaid flowchart of the RS485 signal path

## Open Investigation Threads

- Determining boot mode pin configuration and what memory is mapped at 0x0000
- Reading/dumping the external I2C EEPROM (BL24C512A) via CH341A programmer
- Analyzing firmware in Ghidra (interrupt handlers, thunk functions, boot sequence)
- Monitoring RS485 and UART4 signals with logic analyzer during boot
- Understanding internal flash type (NOR vs NAND) for proper OpenOCD read commands

## License

Documentation, analysis notes, diagrams, and images in this repository are licensed under [CC-BY-NC-4.0](LICENSE). Any code, scripts, or tools are licensed under [MIT](LICENSE-CODE).

Datasheets in `datasheets/` are the property of their respective manufacturers and are included for reference purposes only. See [NOTICE.md](NOTICE.md) for full attribution and legal details.

This reverse engineering work is conducted for the purpose of achieving interoperability with [NUT (Network UPS Tools)](https://networkupstools.org/) under applicable interoperability provisions of law.
