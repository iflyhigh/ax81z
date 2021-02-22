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
|Pin|Usage|Comment|
|---|---|---|
|1|GND|Shortcut with 11|
|2|IRQ||
|3|!IC|Reset|
