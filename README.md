Yamaha TX81Z partial reimplementation for Arduino
=================================================

Yamaha TX81Z design
-------------------
Yamaha TX81Z FM Tone Generator is built around Yamaha YM2414B (aka OPZ) sound generation IC which is very similar to popular, well-documented and even reversed (see https://github.com/nukeykt/Nuked-OPM) YM2151 (aka OPM) IC. CPU is Hitachi HD63B03XP which is Motorola 6800 derivative. TX81Z's ROM size is 64K thus it occupies the whole address space of CPU, so Yamaha engineers invented a simple trick to be able to use external RAM: the MSB (A15) bit of CPU addrress bus is not directly connected to A15 pin of ROM IC, but rather drives ROM IC's CE input pin (Chip Enable), and ROM IC's pin A15 is independently driven by P63 output of CPU. At the same time A13, A14 and A15 outputs of CPU are connected to address decoder IC (SN74HC138N) with the following address map:

|Address range|Component|
|----------|------|
|0x2000 - 0x3FFF|OPZ|
|0x4000 - 0x5FFF|LCD|
|0x6000 - 0x7FFF|RAM|
