# Yamaha TX81Z partial reimplementation for Arduino

## TX81Z design notes

Yamaha TX81Z FM Tone Generator is built around Yamaha YM2414B (aka OPZ) sound generation IC which is very similar to popular, well-documented and even reversed (see https://github.com/nukeykt/Nuked-OPM) YM2151 (aka OPM) IC. CPU is Hitachi HD63B03XP which is Motorola 6800 derivative. TX81Z's ROM size is 64K thus it occupies the whole address space of CPU, so Yamaha engineers invented a simple trick to be able to use external RAM: the MSB (A15) bit of CPU addrress bus is not directly connected to A15 pin of ROM IC, but rather drives ROM IC's CE input pin (Chip Enable), and ROM IC's pin A15 is independently driven by P63 and RD outputs of CPU. In parallel to ROM IC A13, A14 and A15 outputs of CPU are connected to address decoder IC (SN74HC138N) with the following address map:

|Address range|Component|
|----------|------|
|0x2000 - 0x3FFF|OPZ|
|0x4000 - 0x5FFF|LCD|
|0x6000 - 0x7FFF|RAM|

So, when CPU P63 is HIGH (and it is set to HIGH during bootup), any address in range 0x8000 - 0xFFFF will go directly to ROM because A15 is set to HIGH thus enabling ROM IC during reads (RD is active). In case P63 is high but address on a bus is 0x7FFF or below, the A15 will go LOW thus disabling CE on ROM IC and address decoder will do it's work enabling OPZ or LCD or RAM depending on address range. However if address is 0x8000 or higher *and* P63 is low the ROM IC will be active but the reads will be performed from lower half of ROM IC - because ROM IC's A15 input is connected to CPU's P63 (which is LOW). Fortunately there's not a lot of code in lower half of ROM as disassembling it is a bit tricky.

TX81Z has several memory buffers of interest: 
* VMEM and AMEM are used to store patch (i.e. instrument) data as it is loaded from media (ROM or tape). VMEM data is common to DX100 and similar synths using YM2164 (aka OPP, the derivative of YM2151/OPM), AMEM holds parameters unique to OPZ.
* VCED and ACED are used to store patch data in "uncompressed" format suitable for editing. Also all values written to OPZ from VCED/ACED and not from VMEM/AMEM.
* PMEM is used to load so called "performance data", i.e. multi-instrument setups.
* PCED is "uncompressed" editable PMEM.

All these memory buffers are documented in TX81Z service manual however user-level documentation is somewhat not very clear and a bit hard to match service manual so I've made attempt to document everything as complete and clear as possible.

## YM2414B/OPZ manual

OPZ uses exactly the same physical layout and very similar register map as YM2151/OPM. Electrical characteristics are not published but I believe they are the same to YM2151/OPM.

### YM2414B/OPZ pinout

Tilde (~) used to show pins with LOW active state

|Pin|Usage|Comment|
|-----|-----|-----|
|1|GND|Shortcut with 11|
|2|~IRQ||
|3|~IC|Reset IC when LOW|
|4|A0|A0 LOW means data on data bus is register address, A0 HIGH means it is register data. Connected to CPU address bus bit 0 (A0)|
|5|~WR|Write to IC when LOW|
|6|~RD|Read from IC when LOW|
|7|~CS|Chip select when LOW|
|8|CT1|Control terminal 1, unused|
|9|CT2|Control terminal 2, unused|
|10|D0|Data bus bit 0|
|11|GND|Shortcut with 1|
|12-18|D1-D7|Data bus bits 1-7|
|19|SH2|Sample-and-hold port 2 to DAC IC YM3012 pin 6|
|20|SH1|Sample-and-hold port 1 to DAC IC YM3012 pin 5|
|21|SO|Serial output to DAC IC YM3012 pin 4|
|22|Vcc|+5V power|
|23|Ø1|Clock output to DAC IC YM3012 pin 2|
|24|ØM|Clock input 3.579545 MHz|

### YM2414B/OPZ register map

OPZ register map is very similar to OPM.

|Address|b7|b6|b5|b4|b3|b2|b1|b0|Comment|Explanation|
|---:|---|---|---|---|---|---|---|---|---------|---------------------------------|
|0x00-0x07|VOL|VOL|VOL|VOL|VOL|VOL|VOL|VOL|Channel 0-7 volume|Unclear and unused|
|0x08|-|OP|OP|OP|OP|CH|CH|CH|Key ON/OFF|4 higher bits are operators, 3 lower - channel number. Key On event in OPZ is triggered when any OP for specific channel changes it's value from 0 to 1, Key Off triggered when operator changes value from 1 to 0. Normally all operators are running and there is no ability to switch operators on/off in VMEM/AMEM, however VCED has parameter 93 that allows to turn on/off specific operator but it is not documented and no menu item is present in TX81Z to change this setting. In other words, to set Key On for a channel one needs to write 0x78 \| channel_number to this register, to set Key Off write 0x00\|channel_number.|



|...|
