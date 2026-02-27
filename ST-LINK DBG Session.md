Info : STLINK V2J37S7 (API v2) VID:PID 0483:3748
Info : Target voltage: 3.298396
Info : clock speed 100 kHz
Info : stlink_dap_op_connect(connect)
Info : SWD DPIDR 0x0bb11477


Documentation on chip cpu id: 0xbb11477
https://developer.arm.com/documentation/ka001237/latest/
What is the ID Code of a Cortex-M0 or Cortex-M0+ DAP
ARMv6M architecture

PARTNO 0xBB indicates that this DP is designed by ARM, conforms to architecture version 1 and uses the Minimal Debug Port (MinDP) implementation.  It is a Cortex-M0 processor
little endian mode

https://developer.arm.com/documentation/ddi0432/c/debug/about-debug/cortex-m0-rom-table-identification-and-entries?lang=en

When reading out memory, I see data from 0x0 - 0xf4e0

* need to figure out if SoC has NAND or NOR Flash.  two different commands to use in openocd for reading it out.
* Need to read up on arm cortex-m0 boot process.  What does the processor execute when its first turned on?


current mode of arm processor:
	arm core_state
	core state: Thumb


---
register listing from 'reg' command:
> halt
[chip.cpu] halted due to debug-request, current mode: Thread 
xPSR: 0x81000000 pc: 0x0000e448 msp: 0x20001238
> reg
===== arm v7m registers
(0) r0 (/32): 0x0000022b
(1) r1 (/32): 0x00000002
(2) r2 (/32): 0x00000320
(3) r3 (/32): 0x00000002
(4) r4 (/32): 0x0000f42c
(5) r5 (/32): 0x00000001
(6) r6 (/32): 0x0000f42c
(7) r7 (/32): 0x00000002
(8) r8 (/32): 0xfbfffff6
(9) r9 (/32): 0xffffffee
(10) r10 (/32): 0xf3ffffdf
(11) r11 (/32): 0x7fffffff
(12) r12 (/32): 0x00000000
(13) sp (/32): 0x20001238
(14) lr (/32): 0x0000632b
(15) pc (/32): 0x0000e448
(16) xpsr (/32): 0x81000000
(17) msp (/32): 0x20001238
(18) psp (/32): 0xfefbfffc
(20) primask (/1): 0x00
(21) basepri (/8): 0x00
(22) faultmask (/1): 0x00
(23) control (/3): 0x00
===== Cortex-M DWT registers
(60) dwt_ctrl (/32)
(61) dwt_cyccnt (/32)
(62) dwt_0_comp (/32)
(63) dwt_0_mask (/4)
(64) dwt_0_function (/32)



------
custom openocd with fudan chip knowledge

shane@bench:~/projects/openocd-code$ ./src/openocd -d -f ../fm033026n_openocd/stlink.cfg 
Open On-Chip Debugger 0.12.0+dev-00873-g1f3f63569-dirty (2025-02-20-21:47)
Licensed under GNU GPL v2
For bug reports, read
	http://openocd.org/doc/doxygen/bugs.html
User : 3 2 options.c:52 configuration_output_handler(): debug_level: 3User : 4 2 options.c:52 configuration_output_handler(): 
Debug: 5 2 options.c:348 parse_cmdline_args(): ARGV[0] = "./src/openocd"
Debug: 6 2 options.c:348 parse_cmdline_args(): ARGV[1] = "-d"
Debug: 7 2 options.c:348 parse_cmdline_args(): ARGV[2] = "-f"
Debug: 8 2 options.c:348 parse_cmdline_args(): ARGV[3] = "../fm033026n_openocd/stlink.cfg"
Debug: 9 2 options.c:233 add_default_dirs(): bindir=/usr/local/bin
Debug: 10 2 options.c:234 add_default_dirs(): pkgdatadir=/usr/local/share/openocd
Debug: 11 2 options.c:235 add_default_dirs(): exepath=/home/shane/projects/openocd-code/src
Debug: 12 2 options.c:236 add_default_dirs(): bin2data=../share/openocd
Debug: 13 2 configuration.c:33 add_script_search_dir(): adding /home/shane/.config/openocd
Debug: 14 2 configuration.c:33 add_script_search_dir(): adding /home/shane/.openocd
Debug: 15 2 configuration.c:33 add_script_search_dir(): adding /home/shane/projects/openocd-code/src/../share/openocd/site
Debug: 16 2 configuration.c:33 add_script_search_dir(): adding /home/shane/projects/openocd-code/src/../share/openocd/scripts
Debug: 17 2 command.c:153 script_debug(): command - ocd_find ../fm033026n_openocd/stlink.cfg
Debug: 18 2 configuration.c:88 find_file(): found ../fm033026n_openocd/stlink.cfg
Debug: 19 2 command.c:153 script_debug(): command - adapter driver st-link
Debug: 20 2 command.c:153 script_debug(): command - st-link vid_pid 0x0483 0x3744 0x0483 0x3748 0x0483 0x374b 0x0483 0x374d 0x0483 0x374e 0x0483 0x374f 0x0483 0x3752 0x0483 0x3753 0x0483 0x3754 0x0483 0x3755 0x0483 0x3757
Debug: 21 2 command.c:153 script_debug(): command - transport select dapdirect_swd
Debug: 22 2 adi_v5_dapdirect.c:187 dapdirect_swd_select(): dapdirect_swd_select()
Debug: 23 2 command.c:153 script_debug(): command - adapter speed 100
Debug: 24 2 adapter.c:253 adapter_config_khz(): handle adapter khz
Debug: 25 2 adapter.c:216 adapter_khz_to_speed(): convert khz to adapter specific speed value
Debug: 26 2 adapter.c:216 adapter_khz_to_speed(): convert khz to adapter specific speed value
Debug: 27 2 command.c:153 script_debug(): command - swd newdap chip cpu -enable
Debug: 28 2 tcl.c:404 handle_jtag_newtap_args(): Creating New Tap, Chip: chip, Tap: cpu, Dotted: chip.cpu, 1 params
Debug: 29 2 core.c:1488 jtag_tap_init(): Created Tap: chip.cpu @ abs position 0, irlen 0, capture: 0x1 mask: 0x3
Debug: 30 2 command.c:153 script_debug(): command - dap create chip.dap -chain-position chip.cpu
Debug: 31 2 command.c:153 script_debug(): command - target create chip.cpu cortex_m -dap chip.dap
Debug: 32 2 command.c:259 register_command(): command 'tpiu' is already registered
Debug: 33 2 command.c:259 register_command(): command 'rtt' is already registered
Debug: 34 2 command.c:153 script_debug(): command - init
Debug: 35 2 command.c:153 script_debug(): command - target init
Debug: 36 2 command.c:153 script_debug(): command - target names
Debug: 37 2 command.c:153 script_debug(): command - chip.cpu cget -event gdb-flash-erase-start
Debug: 38 2 command.c:153 script_debug(): command - chip.cpu configure -event gdb-flash-erase-start reset init
Debug: 39 2 command.c:153 script_debug(): command - chip.cpu cget -event gdb-flash-write-end
Debug: 40 2 command.c:153 script_debug(): command - chip.cpu configure -event gdb-flash-write-end reset halt
Debug: 41 2 command.c:153 script_debug(): command - chip.cpu cget -event gdb-attach
Debug: 42 2 command.c:153 script_debug(): command - chip.cpu configure -event gdb-attach halt 1000
Debug: 43 2 target.c:1588 handle_target_init_command(): Initializing targets...
Debug: 44 2 semihosting_common.c:109 semihosting_common_init():  
Debug: 45 2 stlink_usb.c:5120 stlink_dap_init(): stlink_dap_init()
Debug: 46 2 stlink_usb.c:3732 stlink_open(): stlink_open
Debug: 47 2 stlink_usb.c:3744 stlink_open(): transport: 4 vid: 0x0483 pid: 0x3744 serial: 
Debug: 48 2 stlink_usb.c:3744 stlink_open(): transport: 4 vid: 0x0483 pid: 0x3748 serial: 
Debug: 49 2 stlink_usb.c:3744 stlink_open(): transport: 4 vid: 0x0483 pid: 0x374b serial: 
Debug: 50 2 stlink_usb.c:3744 stlink_open(): transport: 4 vid: 0x0483 pid: 0x374d serial: 
Debug: 51 2 stlink_usb.c:3744 stlink_open(): transport: 4 vid: 0x0483 pid: 0x374e serial: 
Debug: 52 2 stlink_usb.c:3744 stlink_open(): transport: 4 vid: 0x0483 pid: 0x374f serial: 
Debug: 53 2 stlink_usb.c:3744 stlink_open(): transport: 4 vid: 0x0483 pid: 0x3752 serial: 
Debug: 54 2 stlink_usb.c:3744 stlink_open(): transport: 4 vid: 0x0483 pid: 0x3753 serial: 
Debug: 55 2 stlink_usb.c:3744 stlink_open(): transport: 4 vid: 0x0483 pid: 0x3754 serial: 
Debug: 56 2 stlink_usb.c:3744 stlink_open(): transport: 4 vid: 0x0483 pid: 0x3755 serial: 
Debug: 57 2 stlink_usb.c:3744 stlink_open(): transport: 4 vid: 0x0483 pid: 0x3757 serial: 
Info : 58 8 stlink_usb.c:1471 stlink_usb_version(): STLINK V2J37S7 (API v2) VID:PID 0483:3748
Debug: 59 8 stlink_usb.c:1696 stlink_usb_exit_mode(): MODE: 0x01
Info : 60 8 stlink_usb.c:1507 stlink_usb_check_voltage(): Target voltage: 3.298396
Debug: 61 8 stlink_usb.c:1764 stlink_usb_init_mode(): MODE: 0x01
Debug: 62 8 stlink_usb.c:3130 stlink_dump_speed_map(): Supported clock speeds are:
Debug: 63 8 stlink_usb.c:3133 stlink_dump_speed_map(): 4000 kHz
Debug: 64 8 stlink_usb.c:3133 stlink_dump_speed_map(): 1800 kHz
Debug: 65 8 stlink_usb.c:3133 stlink_dump_speed_map(): 1200 kHz
Debug: 66 8 stlink_usb.c:3133 stlink_dump_speed_map(): 950 kHz
Debug: 67 8 stlink_usb.c:3133 stlink_dump_speed_map(): 480 kHz
Debug: 68 8 stlink_usb.c:3133 stlink_dump_speed_map(): 240 kHz
Debug: 69 8 stlink_usb.c:3133 stlink_dump_speed_map(): 125 kHz
Debug: 70 8 stlink_usb.c:3133 stlink_dump_speed_map(): 100 kHz
Debug: 71 8 stlink_usb.c:3133 stlink_dump_speed_map(): 50 kHz
Debug: 72 8 stlink_usb.c:3133 stlink_dump_speed_map(): 25 kHz
Debug: 73 8 stlink_usb.c:3133 stlink_dump_speed_map(): 15 kHz
Debug: 74 8 stlink_usb.c:3133 stlink_dump_speed_map(): 5 kHz
Debug: 75 26 stlink_usb.c:1824 stlink_usb_init_mode(): MODE: 0x02
Debug: 76 28 stlink_usb.c:4127 stlink_usb_open_ap(): AP 0 enabled
Debug: 77 30 stlink_usb.c:3820 stlink_open(): Using TAR autoincrement: 1024
Debug: 78 30 adapter.c:216 adapter_khz_to_speed(): convert khz to adapter specific speed value
Debug: 79 30 adapter.c:220 adapter_khz_to_speed(): have adapter set up
Debug: 80 30 adapter.c:216 adapter_khz_to_speed(): convert khz to adapter specific speed value
Debug: 81 30 adapter.c:220 adapter_khz_to_speed(): have adapter set up
Info : 82 30 adapter.c:180 adapter_init(): clock speed 100 kHz
Debug: 83 30 openocd.c:133 handle_init_command(): Debug Adapter init complete
Debug: 84 30 command.c:153 script_debug(): command - transport init
Debug: 85 30 transport.c:219 handle_transport_init(): handle_transport_init
Debug: 86 30 adi_v5_dapdirect.c:196 dapdirect_init(): dapdirect_init()
Debug: 87 30 stlink_usb.c:5163 stlink_dap_reset(): stlink_dap_reset(0)
Debug: 88 30 core.c:653 adapter_system_reset(): SRST line released
Debug: 89 30 command.c:153 script_debug(): command - dap init
Debug: 90 30 arm_dap.c:96 dap_init_all(): Initializing all DAPs ...
Debug: 91 30 arm_dap.c:120 dap_init_all(): DAP chip.cpu configured by default to use ADIv5 protocol
Info : 92 30 stlink_usb.c:4197 stlink_dap_op_connect(): stlink_dap_op_connect(connect)
Debug: 93 30 arm_adi_v5.c:783 dap_dp_init(): chip.dap
Debug: 94 30 arm_adi_v5.c:816 dap_dp_init(): DAP: wait CDBGPWRUPACK
Debug: 95 30 arm_adi_v5.h:682 dap_dp_poll_register(): DAP: poll 4, mask 0x20000000, value 0x20000000
Debug: 96 33 arm_adi_v5.c:824 dap_dp_init(): DAP: wait CSYSPWRUPACK
Debug: 97 33 arm_adi_v5.h:682 dap_dp_poll_register(): DAP: poll 4, mask 0x80000000, value 0x80000000
Debug: 98 36 stlink_usb.c:2057 stlink_usb_idcode(): IDCODE: 0x0BB11477
Info : 99 36 stlink_usb.c:4224 stlink_dap_op_connect(): SWD DPIDR 0x0bb11477
Debug: 100 36 openocd.c:150 handle_init_command(): Examining targets...
Debug: 101 36 target.c:674 target_examine_one(): [chip.cpu] Examination started
Debug: 102 36 target.c:1774 target_call_event_callbacks(): target event 19 (examine-start) for core chip.cpu
Debug: 103 36 arm_adi_v5.c:1193 dap_get_ap(): refcount AP#0x0 get 1
Debug: 104 37 arm_adi_v5.c:1136 dap_find_get_ap(): Found MEM-AP AHB3 at AP index: 0 (IDR=0x04770021)
Debug: 105 38 arm_adi_v5.c:934 mem_ap_init(): MEM_AP CFG: large data 0, long address 0, big-endian 0
Debug: 106 42 target.c:2562 target_read_u32(): address: 0xe000ed00, value: 0x410cc300
Info : 107 42 cortex_m.c:2650 cortex_m_examine(): [chip.cpu] Cortex-M0 r0p0 processor detected
Debug: 108 42 cortex_m.c:2673 cortex_m_examine(): [chip.cpu] cpuid: 0x410cc300
Debug: 109 44 target.c:2562 target_read_u32(): address: 0xe000edf0, value: 0x01000000
Debug: 110 44 target.c:2650 target_write_u32(): address: 0xe000edf0, value: 0xa05f0001
Debug: 111 46 target.c:2650 target_write_u32(): address: 0xe000edfc, value: 0x01000000
Debug: 112 48 target.c:2650 target_write_u32(): address: 0xe0000fb0, value: 0xc5acce55
Debug: 113 52 target.c:2562 target_read_u32(): address: 0xe0000e80, value: 0x00000000
Debug: 114 52 target.c:2650 target_write_u32(): address: 0xe0000e80, value: 0x00000000
Debug: 115 56 target.c:2562 target_read_u32(): address: 0xe0000e80, value: 0x00000000
Debug: 116 56 target.c:2650 target_write_u32(): address: 0xe0000e80, value: 0x00010009
Debug: 117 58 target.c:2650 target_write_u32(): address: 0xe0000e00, value: 0x00000001
Debug: 118 60 target.c:2650 target_write_u32(): address: 0xe0000e04, value: 0x00000000
Debug: 119 62 target.c:2650 target_write_u32(): address: 0xe0000e08, value: 0x00000000
Debug: 120 64 target.c:2650 target_write_u32(): address: 0xe0000e0c, value: 0x00000000
Debug: 121 66 target.c:2650 target_write_u32(): address: 0xe0000e10, value: 0x00000000
Debug: 122 68 target.c:2650 target_write_u32(): address: 0xe0000e14, value: 0x00000000
Debug: 123 70 target.c:2650 target_write_u32(): address: 0xe0000e18, value: 0x00000000
Debug: 124 72 target.c:2650 target_write_u32(): address: 0xe0000e1c, value: 0x00000000
Debug: 125 76 target.c:2562 target_read_u32(): address: 0xe0002000, value: 0x00000040
Debug: 126 76 target.c:2650 target_write_u32(): address: 0xe0002008, value: 0x00000000
Debug: 127 78 target.c:2650 target_write_u32(): address: 0xe000200c, value: 0x00000000
Debug: 128 80 target.c:2650 target_write_u32(): address: 0xe0002010, value: 0x00000000
Debug: 129 82 target.c:2650 target_write_u32(): address: 0xe0002014, value: 0x00000000
Debug: 130 84 cortex_m.c:2793 cortex_m_examine(): [chip.cpu] FPB fpcr 0x40, numcode 4, numlit 0
Debug: 131 87 target.c:2562 target_read_u32(): address: 0xe0001000, value: 0x10000000
Debug: 132 87 cortex_m.c:2467 cortex_m_dwt_setup(): [chip.cpu] DWT_CTRL: 0x10000000
Debug: 133 89 target.c:2562 target_read_u32(): address: 0xe0001fbc, value: 0x00000000
Debug: 134 89 cortex_m.c:2474 cortex_m_dwt_setup(): [chip.cpu] DWT_DEVARCH: 0x0
Debug: 135 89 target.c:2650 target_write_u32(): address: 0xe0001028, value: 0x00000000
Debug: 136 91 cortex_m.c:2521 cortex_m_dwt_setup(): [chip.cpu] DWT dwtcr 0x10000000, comp 1, watch/trigger
Info : 137 91 cortex_m.c:2803 cortex_m_examine(): [chip.cpu] target has 4 breakpoints, 1 watchpoints
Debug: 138 91 target.c:1774 target_call_event_callbacks(): target event 21 (examine-end) for core chip.cpu
Info : 139 91 target.c:690 target_examine_one(): [chip.cpu] Examination succeed
Debug: 140 91 command.c:153 script_debug(): command - flash init
Debug: 141 91 tcl.c:1364 handle_flash_init_command(): Initializing flash devices...
Debug: 142 91 command.c:153 script_debug(): command - nand init
Debug: 143 91 tcl.c:484 handle_nand_init_command(): Initializing NAND devices...
Debug: 144 91 command.c:153 script_debug(): command - pld init
Debug: 145 91 pld.c:337 handle_pld_init_command(): Initializing PLDs...
Debug: 146 91 command.c:153 script_debug(): command - tpiu init
Info : 147 91 gdb_server.c:3831 gdb_target_start(): [chip.cpu] starting gdb server on 3333
Info : 148 91 server.c:298 add_service(): Listening on port 3333 for gdb connections
Debug: 149 91 command.c:153 script_debug(): command - target names
Debug: 150 91 command.c:153 script_debug(): command - target names
Debug: 151 91 command.c:153 script_debug(): command - chip.cpu cget -type
Debug: 152 91 command.c:153 script_debug(): command - dap info
Debug: 153 91 arm_adi_v5.c:1193 dap_get_ap(): refcount AP#0x0 get 2
Debug: 154 94 stlink_usb.c:4377 stlink_usb_misc_rw_segment(): Queue: 12 commands in 13 items
Debug: 155 113 stlink_usb.c:4377 stlink_usb_misc_rw_segment(): Queue: 12 commands in 13 items
Debug: 156 132 stlink_usb.c:4377 stlink_usb_misc_rw_segment(): Queue: 12 commands in 13 items
Debug: 157 151 stlink_usb.c:4377 stlink_usb_misc_rw_segment(): Queue: 12 commands in 13 items
Debug: 158 170 arm_adi_v5.c:1218 dap_put_ap(): refcount AP#0x0 put 1
User : 159 170 options.c:52 configuration_output_handler(): AP # 0x0
		AP ID register 0x04770021
		Type is MEM-AP AHB3
MEM-AP BASE 0xe00ff003
		Valid ROM table present
		Component base address 0xe00ff000
		Peripheral ID 0x04000bb471
		Designer is 0x23b, ARM Ltd
		Part is 0x471, Cortex-M0 ROM (ROM Table)
		Component class is 0x1, ROM table
		MEMTYPE system memory present on bus
	ROMTABLE[0x0] = 0xfff0f003
		Component base address 0xe000e000
		Peripheral ID 0x04000bb00d
		Designer is 0x23b, ARM Ltd
		Part is 0x00d, CoreSight ETM11 (Embedded Trace)
		Component class is 0xe, Generic IP component
	ROMTABLE[0x4] = 0xfff02003
		Component base address 0xe0001000
		Peripheral ID 0x04000bb00a
		Designer is 0x23b, ARM Ltd
		Part is 0x00a, Cortex-M0 DWT (Data Watchpoint and Trace)
		Component class is 0xe, Generic IP component
	ROMTABLE[0x8] = 0xfff03003
		Component base address 0xe0002000
		Peripheral ID 0x04000bb00b
		Designer is 0x23b, ARM Ltd
		Part is 0x00b, Cortex-M0 BPU (Breakpoint Unit)
		Component class is 0xe, Generic IP component
	ROMTABLE[0xc] = 0x00000000
		End of ROM tableUser : 160 170 options.c:52 configuration_output_handler(): 
Info : 161 170 server.c:298 add_service(): Listening on port 6666 for tcl connections
Info : 162 170 server.c:298 add_service(): Listening on port 4444 for telnet connections
Debug: 163 170 command.c:153 script_debug(): command - init






------

Info : STLINK V2J37S7 (API v2) VID:PID 0483:3748
Info : Target voltage: 3.298396
Info : clock speed 100 kHz
Info : stlink_dap_op_connect(connect)
Info : SWD DPIDR 0x0bb11477
Error: [chip.cpu] Cortex-M PARTNO 0xc30 is unrecognized
Warn : target chip.cpu examination failed
Info : starting gdb server for chip.cpu on 3333
Info : Listening on port 3333 for gdb connections
AP # 0x0
		AP ID register 0x04770021
		Type is MEM-AP AHB3
MEM-AP BASE 0xe00ff003
		Valid ROM table present
		Component base address 0xe00ff000
		Peripheral ID 0x04000bb471
		Designer is 0x23b, ARM Ltd
		Part is 0x471, Cortex-M0 ROM (ROM Table)
		Component class is 0x1, ROM table
		MEMTYPE system memory present on bus
	ROMTABLE[0x0] = 0xfff0f003
		Component base address 0xe000e000
		Peripheral ID 0x04000bb00d
		Designer is 0x23b, ARM Ltd
		Part is 0x00d, CoreSight ETM11 (Embedded Trace)
		Component class is 0xe, Generic IP component
	ROMTABLE[0x4] = 0xfff02003
		Component base address 0xe0001000
		Peripheral ID 0x04000bb00a
		Designer is 0x23b, ARM Ltd
		Part is 0x00a, Cortex-M0 DWT (Data Watchpoint and Trace)
		Component class is 0xe, Generic IP component
	ROMTABLE[0x8] = 0xfff03003
		Component base address 0xe0002000
		Peripheral ID 0x04000bb00b
		Designer is 0x23b, ARM Ltd
		Part is 0x00b, Cortex-M0 BPU (Breakpoint Unit)
		Component class is 0xe, Generic IP component
	ROMTABLE[0xc] = 0x00000000
		End of ROM table

Info : Listening on port 6666 for tcl connections
Info : Listening on port 4444 for telnet connections



------
debug level 3 run


Open On-Chip Debugger 0.12.0
Licensed under GNU GPL v2
For bug reports, read
	http://openocd.org/doc/doxygen/bugs.html
User : 3 2 options.c:52 configuration_output_handler(): debug_level: 3
User : 4 2 options.c:52 configuration_output_handler(): 
Debug: 5 2 options.c:233 add_default_dirs(): bindir=/usr/bin
Debug: 6 2 options.c:234 add_default_dirs(): pkgdatadir=/usr/share/openocd
Debug: 7 2 options.c:235 add_default_dirs(): exepath=/usr/bin
Debug: 8 2 options.c:236 add_default_dirs(): bin2data=../share/openocd
Debug: 9 2 configuration.c:33 add_script_search_dir(): adding /root/.config/openocd
Debug: 10 2 configuration.c:33 add_script_search_dir(): adding /root/.openocd
Debug: 11 2 configuration.c:33 add_script_search_dir(): adding /usr/bin/../share/openocd/site
Debug: 12 2 configuration.c:33 add_script_search_dir(): adding /usr/bin/../share/openocd/scripts
Debug: 13 2 command.c:155 script_debug(): command - ocd_find stlink.cfg
Debug: 14 3 configuration.c:88 find_file(): found stlink.cfg
Debug: 15 3 command.c:155 script_debug(): command - adapter driver st-link
Debug: 16 3 command.c:155 script_debug(): command - st-link vid_pid 0x0483 0x3744 0x0483 0x3748 0x0483 0x374b 0x0483 0x374d 0x0483 0x374e 0x0483 0x374f 0x0483 0x3752 0x0483 0x3753 0x0483 0x3754 0x0483 0x3755 0x0483 0x3757
Debug: 17 3 command.c:155 script_debug(): command - transport select dapdirect_swd
Debug: 18 3 adi_v5_dapdirect.c:164 dapdirect_swd_select(): dapdirect_swd_select()
Debug: 19 3 command.c:155 script_debug(): command - adapter speed 100
Debug: 20 3 adapter.c:251 adapter_config_khz(): handle adapter khz
Debug: 21 3 adapter.c:215 adapter_khz_to_speed(): convert khz to adapter specific speed value
Debug: 22 3 adapter.c:215 adapter_khz_to_speed(): convert khz to adapter specific speed value
Debug: 23 3 command.c:155 script_debug(): command - swd newdap chip cpu -enable
Debug: 24 3 tcl.c:557 jim_newtap_cmd(): Creating New Tap, Chip: chip, Tap: cpu, Dotted: chip.cpu, 1 params
Debug: 25 3 core.c:1474 jtag_tap_init(): Created Tap: chip.cpu @ abs position 0, irlen 0, capture: 0x0 mask: 0x0
Debug: 26 3 command.c:155 script_debug(): command - dap create chip.dap -chain-position chip.cpu
Debug: 27 3 command.c:155 script_debug(): command - target create chip.cpu cortex_m -dap chip.dap
Debug: 28 3 command.c:289 register_command(): command 'tpiu' is already registered
Debug: 29 3 command.c:289 register_command(): command 'rtt' is already registered
Debug: 30 3 command.c:155 script_debug(): command - init
Debug: 31 3 command.c:155 script_debug(): command - target init
Debug: 32 3 command.c:155 script_debug(): command - target names
Debug: 33 3 command.c:155 script_debug(): command - chip.cpu cget -event gdb-flash-erase-start
Debug: 34 3 command.c:155 script_debug(): command - chip.cpu configure -event gdb-flash-erase-start reset init
Debug: 35 3 command.c:155 script_debug(): command - chip.cpu cget -event gdb-flash-write-end
Debug: 36 3 command.c:155 script_debug(): command - chip.cpu configure -event gdb-flash-write-end reset halt
Debug: 37 3 command.c:155 script_debug(): command - chip.cpu cget -event gdb-attach
Debug: 38 3 command.c:155 script_debug(): command - chip.cpu configure -event gdb-attach halt 1000
Debug: 39 3 target.c:1657 handle_target_init_command(): Initializing targets...
Debug: 40 3 semihosting_common.c:109 semihosting_common_init():  
Debug: 41 3 stlink_usb.c:5081 stlink_dap_init(): stlink_dap_init()
Debug: 42 3 stlink_usb.c:3693 stlink_open(): stlink_open
Debug: 43 3 stlink_usb.c:3705 stlink_open(): transport: 4 vid: 0x0483 pid: 0x3744 serial: 
Debug: 44 3 stlink_usb.c:3705 stlink_open(): transport: 4 vid: 0x0483 pid: 0x3748 serial: 
Debug: 45 3 stlink_usb.c:3705 stlink_open(): transport: 4 vid: 0x0483 pid: 0x374b serial: 
Debug: 46 3 stlink_usb.c:3705 stlink_open(): transport: 4 vid: 0x0483 pid: 0x374d serial: 
Debug: 47 3 stlink_usb.c:3705 stlink_open(): transport: 4 vid: 0x0483 pid: 0x374e serial: 
Debug: 48 3 stlink_usb.c:3705 stlink_open(): transport: 4 vid: 0x0483 pid: 0x374f serial: 
Debug: 49 3 stlink_usb.c:3705 stlink_open(): transport: 4 vid: 0x0483 pid: 0x3752 serial: 
Debug: 50 3 stlink_usb.c:3705 stlink_open(): transport: 4 vid: 0x0483 pid: 0x3753 serial: 
Debug: 51 3 stlink_usb.c:3705 stlink_open(): transport: 4 vid: 0x0483 pid: 0x3754 serial: 
Debug: 52 3 stlink_usb.c:3705 stlink_open(): transport: 4 vid: 0x0483 pid: 0x3755 serial: 
Debug: 53 3 stlink_usb.c:3705 stlink_open(): transport: 4 vid: 0x0483 pid: 0x3757 serial: 
Info : 54 9 stlink_usb.c:1434 stlink_usb_version(): STLINK V2J37S7 (API v2) VID:PID 0483:3748
Debug: 55 9 stlink_usb.c:1659 stlink_usb_exit_mode(): MODE: 0x01
Info : 56 9 stlink_usb.c:1470 stlink_usb_check_voltage(): Target voltage: 3.298396
Debug: 57 9 stlink_usb.c:1727 stlink_usb_init_mode(): MODE: 0x01
Debug: 58 9 stlink_usb.c:3093 stlink_dump_speed_map(): Supported clock speeds are:
Debug: 59 9 stlink_usb.c:3096 stlink_dump_speed_map(): 4000 kHz
Debug: 60 9 stlink_usb.c:3096 stlink_dump_speed_map(): 1800 kHz
Debug: 61 9 stlink_usb.c:3096 stlink_dump_speed_map(): 1200 kHz
Debug: 62 9 stlink_usb.c:3096 stlink_dump_speed_map(): 950 kHz
Debug: 63 9 stlink_usb.c:3096 stlink_dump_speed_map(): 480 kHz
Debug: 64 9 stlink_usb.c:3096 stlink_dump_speed_map(): 240 kHz
Debug: 65 9 stlink_usb.c:3096 stlink_dump_speed_map(): 125 kHz
Debug: 66 9 stlink_usb.c:3096 stlink_dump_speed_map(): 100 kHz
Debug: 67 9 stlink_usb.c:3096 stlink_dump_speed_map(): 50 kHz
Debug: 68 9 stlink_usb.c:3096 stlink_dump_speed_map(): 25 kHz
Debug: 69 9 stlink_usb.c:3096 stlink_dump_speed_map(): 15 kHz
Debug: 70 9 stlink_usb.c:3096 stlink_dump_speed_map(): 5 kHz
Debug: 71 337 stlink_usb.c:1787 stlink_usb_init_mode(): MODE: 0x02
Debug: 72 362 stlink_usb.c:4088 stlink_usb_open_ap(): AP 0 enabled
Debug: 73 401 stlink_usb.c:3781 stlink_open(): Using TAR autoincrement: 1024
Debug: 74 401 adapter.c:215 adapter_khz_to_speed(): convert khz to adapter specific speed value
Debug: 75 401 adapter.c:219 adapter_khz_to_speed(): have adapter set up
Debug: 76 401 adapter.c:215 adapter_khz_to_speed(): convert khz to adapter specific speed value
Debug: 77 401 adapter.c:219 adapter_khz_to_speed(): have adapter set up
Info : 78 401 adapter.c:179 adapter_init(): clock speed 100 kHz
Debug: 79 401 openocd.c:134 handle_init_command(): Debug Adapter init complete
Debug: 80 401 command.c:155 script_debug(): command - transport init
Debug: 81 401 transport.c:219 handle_transport_init(): handle_transport_init
Debug: 82 401 adi_v5_dapdirect.c:173 dapdirect_init(): dapdirect_init()
Debug: 83 401 stlink_usb.c:5124 stlink_dap_reset(): stlink_dap_reset(0)
Debug: 84 401 core.c:640 adapter_system_reset(): SRST line released
Debug: 85 401 command.c:155 script_debug(): command - dap init
Debug: 86 401 arm_dap.c:97 dap_init_all(): Initializing all DAPs ...
Debug: 87 401 arm_dap.c:121 dap_init_all(): DAP chip.cpu configured by default to use ADIv5 protocol
Info : 88 401 stlink_usb.c:4158 stlink_dap_op_connect(): stlink_dap_op_connect(connect)
Debug: 89 402 arm_adi_v5.c:679 dap_dp_init(): chip.dap
Debug: 90 402 arm_adi_v5.c:711 dap_dp_init(): DAP: wait CDBGPWRUPACK
Debug: 91 402 arm_adi_v5.h:638 dap_dp_poll_register(): DAP: poll 4, mask 0x20000000, value 0x20000000
Debug: 92 404 arm_adi_v5.c:719 dap_dp_init(): DAP: wait CSYSPWRUPACK
Debug: 93 404 arm_adi_v5.h:638 dap_dp_poll_register(): DAP: poll 4, mask 0x80000000, value 0x80000000
Debug: 94 407 stlink_usb.c:2020 stlink_usb_idcode(): IDCODE: 0x0BB11477
Info : 95 408 stlink_usb.c:4185 stlink_dap_op_connect(): SWD DPIDR 0x0bb11477
Debug: 96 408 openocd.c:151 handle_init_command(): Examining targets...
Debug: 97 408 target.c:1843 target_call_event_callbacks(): target event 19 (examine-start) for core chip.cpu
Debug: 98 408 arm_adi_v5.c:1095 dap_get_ap(): refcount AP#0x0 get 1
Debug: 99 409 arm_adi_v5.c:1038 dap_find_get_ap(): Found MEM-AP AHB3 at AP index: 0 (IDR=0x04770021)
Debug: 100 413 arm_adi_v5.c:825 mem_ap_init(): MEM_AP Packed Transfers: disabled
Debug: 101 413 arm_adi_v5.c:836 mem_ap_init(): MEM_AP CFG: large data 0, long address 0, big-endian 0
Debug: 102 415 target.c:2628 target_read_u32(): address: 0xe000ed00, value: 0x410cc300
Error: 103 415 cortex_m.c:2363 cortex_m_examine(): [chip.cpu] Cortex-M PARTNO 0xc30 is unrecognized
Debug: 104 415 target.c:1843 target_call_event_callbacks(): target event 20 (examine-fail) for core chip.cpu
Warn : 105 415 target.c:802 target_examine(): target chip.cpu examination failed
Debug: 106 415 openocd.c:153 handle_init_command(): target examination failed
Debug: 107 415 command.c:155 script_debug(): command - flash init
Debug: 108 415 tcl.c:1375 handle_flash_init_command(): Initializing flash devices...
Debug: 109 415 command.c:155 script_debug(): command - nand init
Debug: 110 415 tcl.c:487 handle_nand_init_command(): Initializing NAND devices...
Debug: 111 415 command.c:155 script_debug(): command - pld init
Debug: 112 415 pld.c:194 handle_pld_init_command(): Initializing PLDs...
Debug: 113 415 command.c:155 script_debug(): command - tpiu init
Info : 114 415 gdb_server.c:3791 gdb_target_start(): starting gdb server for chip.cpu on 3333
Info : 115 415 server.c:297 add_service(): Listening on port 3333 for gdb connections
Debug: 116 415 command.c:155 script_debug(): command - dap info
Debug: 117 415 arm_adi_v5.c:1095 dap_get_ap(): refcount AP#0x0 get 2
Debug: 118 418 stlink_usb.c:4338 stlink_usb_misc_rw_segment(): Queue: 12 commands in 13 items
Debug: 119 437 stlink_usb.c:4338 stlink_usb_misc_rw_segment(): Queue: 12 commands in 13 items
Debug: 120 456 stlink_usb.c:4338 stlink_usb_misc_rw_segment(): Queue: 12 commands in 13 items
Debug: 121 475 stlink_usb.c:4338 stlink_usb_misc_rw_segment(): Queue: 12 commands in 13 items
Debug: 122 493 arm_adi_v5.c:1120 dap_put_ap(): refcount AP#0x0 put 1
User : 123 493 options.c:52 configuration_output_handler(): AP # 0x0
		AP ID register 0x04770021
		Type is MEM-AP AHB3
MEM-AP BASE 0xe00ff003
		Valid ROM table present
		Component base address 0xe00ff000
		Peripheral ID 0x04000bb471
		Designer is 0x23b, ARM Ltd
		Part is 0x471, Cortex-M0 ROM (ROM Table)
		Component class is 0x1, ROM table
		MEMTYPE system memory present on bus
	ROMTABLE[0x0] = 0xfff0f003
		Component base address 0xe000e000
		Peripheral ID 0x04000bb00d
		Designer is 0x23b, ARM Ltd
		Part is 0x00d, CoreSight ETM11 (Embedded Trace)
		Component class is 0xe, Generic IP component
	ROMTABLE[0x4] = 0xfff02003
		Component base address 0xe0001000
		Peripheral ID 0x04000bb00a
		Designer is 0x23b, ARM Ltd
		Part is 0x00a, Cortex-M0 DWT (Data Watchpoint and Trace)
		Component class is 0xe, Generic IP component
	ROMTABLE[0x8] = 0xfff03003
		Component base address 0xe0002000
		Peripheral ID 0x04000bb00b
		Designer is 0x23b, ARM Ltd
		Part is 0x00b, Cortex-M0 BPU (Breakpoint Unit)
		Component class is 0xe, Generic IP component
	ROMTABLE[0xc] = 0x00000000
		End of ROM table
User : 124 494 options.c:52 configuration_output_handler(): 
Info : 125 494 server.c:297 add_service(): Listening on port 6666 for tcl connections
Info : 126 494 server.c:297 add_service(): Listening on port 4444 for telnet connections
Debug: 127 494 command.c:155 script_debug(): command - init
