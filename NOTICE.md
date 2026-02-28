# Legal Notice

## Purpose and Interoperability Statement

This project documents the reverse engineering of a GoldenMate Battery Management System (BMS) for the purpose of achieving interoperability with [NUT (Network UPS Tools)](https://networkupstools.org/), an open-source power device monitoring framework. The goal is to enable external battery monitoring, status reporting, and integration with existing open-source power management infrastructure.

This work is conducted under the interoperability provisions of applicable law, including Section 103(f) of the Digital Millennium Copyright Act (17 U.S.C. 1201(f)), which permits reverse engineering of a lawfully obtained computer program for the purpose of achieving interoperability with independently created programs.

## Licensing

This repository uses a dual-license structure:

- **Documentation, analysis, diagrams, and images** are licensed under the [Creative Commons Attribution 4.0 International License (CC-BY-4.0)](LICENSE).
- **Code, scripts, and tools** (if present) are licensed under the [MIT License](LICENSE-CODE).

## Third-Party Content

### Datasheets

The `datasheets/` directory contains manufacturer datasheets and translated excerpts included as reference material for this reverse engineering project. These documents are the intellectual property of their respective manufacturers:

| Manufacturer | Components |
|---|---|
| Fudan Microelectronics | FM33LC026N MCU |
| Sinowealth Semiconductor | SH367309 BMS ASIC |
| Shanghai Belling | BL24C512A EEPROM |
| Chipanalog Microelectronics | CA-IS3721HS Isolator |
| 3PEAK Incorporated | TP8485E RS485 Transceiver |
| XLSEMI | XL7015E1 Voltage Regulator |
| AIPULNION | FN1-05S05AN DC-DC Module |
| Jiangsu Changjing Electronics | VBE2102M |
| Nanjing Micro One Electronics | SA6 TVS Diode |
| Shanghai Siproin Microelectronics | SS027N08LS MOSFET |
| Qinheng Microelectronics | CH341A/CH341B Programmer |
| ARM Limited | Cortex-M0 Technical Reference |

These datasheets are included for educational and interoperability research purposes. They remain the property of their respective copyright holders. No endorsement by any manufacturer is implied.

The `datasheets/mcu/chapters-english/` directory contains Chinese-to-English translations of select FM33LC026N datasheet chapters, produced as part of this research effort.

### PCB Photography

Photographs of the GoldenMate BMS circuit board in the `images/` directory were taken by the author of hardware lawfully purchased and owned.

## Disclaimer

This project is an independent research effort. It is not affiliated with, endorsed by, or sponsored by any of the manufacturers listed above. All trademarks and product names are the property of their respective owners.

The information in this repository is provided as-is for educational and interoperability purposes. No warranty is made regarding its accuracy or completeness.
