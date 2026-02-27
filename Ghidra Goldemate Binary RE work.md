
The initial part of mem.bin looks like ghidra is interpreting certain offsets as interrupt handlers or something.

0x140 looks to be an entry off of a sled.

* [x] I need to lookup arm cortex-m0 processors to understand their boot sequence. The flash section of the fudan doc talks a little about this. ✅ 2025-02-22
* [x] I also need to look up ARM asm nemonics and understand what "thumb" mode is. ✅ 2025-02-22
* [x] figure out how to access the Fudan SoC flash.  Is it memory mapped? See the fudan "flash" section document, it discusses addressing the flash. ✅ 2025-02-23
* [ ] how can i tell what mode the processor is in when its halted?  which register?
* [ ] What is a thunk function 
* [ ] what is the code doing while its halted in the debugger?  Is it waiting for something, or stuck in an error loop.
* [ ] Can I tell where the SoC bootloader reads from the external eeprom?

* [ ] desolder flash and try to read it off again now that I know what it is.  Will likely need to modify one of the existing reader sources to make it work with this eeprom.
* [ ] After reading off flash, reassemble the device and power as normal with debugger attached, and logic analyzer on rs485 and PCS UART4 port
* [ ] 


On reset, cpu starts executing the RESET Vector.  This initial program is called the Bootrom.

Need to check the BootMode pins on the processor to understand what memory is mapped to the 0x0000 address.
