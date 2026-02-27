Labeled PCB v2
![[PCB-layout-markup.jpg]]

BMS MCU/SoC: FM33LC026N
rs485 controller: 
- 3PEAK. TP8485E
- CA-IS3721HS

eeprom: BL24C512A  (likely an i2c eeprom)
ASIC BMS: SH367309U

Power supply/regulator: AIPULNION FN1-05**S**05an

VBsemi  VBE2102M  (V4G10)

XLSEMI XL7015E1

r2m cm

cf413nn



- [x] There is about 3 UARTs on the MCU. Need to investigate if they are connected to any thing.  If they are, it might be a console there. ✅ 2024-12-27
	- ~~I'm not finding any reasonable connections.  All the UART pins on the SoC are multipurpose so a continuity trace doesn't imply the pin is actually being used for UART~~
- [x] If there are uart pads, monitor them with a multi-meter to see if there is fluctuating voltage on the TX pin 🔽 ➕ 2024-12-27 ✅ 2024-12-27
- [x] Check to see what pins on the MCU the RS485 controller is connected to.  I'm curious if it would be useful later. ✅ 2024-12-29
	- ~~The pins are not directly connected to the SoC. They go through at least 2 ICs.  Need a microscope to identity one of the ICs.~~
	- ~~I've identified the ICs as "isolators".  Still working on tracing the lines back to the MCU.~~
- [ ] MCU datasheet says there is flash on the SoC.  How do I read this externally?
	- ~~Says it can be written by a programmer provided by Fudan Microelectronics.~~
	- ~~I think this SoC might be accessible via the SWD interface for ARM... Need to research a bit of what I need to connect to this interface.~~
	- I have a device on order that might help use this interface from openocd.
- [ ] connect logical analyzer to rs485 port and monitor signals on boot up.
- [ ] modify ch341a software to identify and read standard i2c stuff from eeprom chip.
- [ ] use windows software for ch341a.

## WIP of pin tracing of 485 to SoC.

![[rs485-trace-mapping.excalidraw]]



For connecting to the SWD debugging interface:
https://openocd.org/
search for: "arm swd debugger" for ICE hardware.
https://developer.arm.com/downloads/view/DS000B The ARM Development Studio is some official software for work with ARM devices.


### rs485 comms

MCU -> CA-IS3721HS -> 3PEAK -> RS485 pins


software

https://www.instructables.com/CH341A-Programmer/

### Stuff to do for OpenOCD/SWD

See data from [[ST-LINK DBG Session]]

* target cpu needs to be defined.  its a cortex_m (m0) but says the manufacturer is ARM where as most chips online use STM as the manufacturer.  I'm not seeing any defined target files for this particular part so I may need to make a target file for it.  I think I can do this from the ARM documentation on the chip, but I'm not sure what needs to be defined yet.
* Internal Flash needs to be defined.  Perhaps I can get this data from Fudan or the datasheet of the SoC.  There is likely internal flash, and an external eeprom chip for the initial loader. The external EEPROM is defined in a "board configuration" file for OpenOCD.  The Internal Flash is defined in the "target configuration" for OpenOCD.

### ARM Cortex-M0 data

SoC part is ARM Cortex-M0   CM0DAO (r0p0)

Technical data sheet: https://developer.arm.com/documentation/ddi0432/c/debug/about-debug/cortex-m0-rom-table-identification-and-entries?lang=en


### External Flash Connections

SCL is on pin 21  (PB3/UART4_TX/ATIM_CH2N)
SDA is on pin 22  (PB4/SEG3/ATIM_CH1)

### LED light

led is on pin 30 (PB12(WKUP3)/ATIM_ETR/FOUT1)