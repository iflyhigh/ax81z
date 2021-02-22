# Yamaha TX81Z partial reimplementation for Arduino

## TX81Z design notes

Yamaha TX81Z FM Tone Generator is built around Yamaha YM2414B (aka OPZ) sound generation IC which is very similar to popular, well-documented and even reversed (see https://github.com/nukeykt/Nuked-OPM) YM2151 (aka OPM) IC. CPU is Hitachi HD63B03XP which is Motorola 6800 derivative. TX81Z's ROM size is 64K thus it occupies the whole address space of CPU, so Yamaha engineers invented a simple trick to be able to use external RAM: the MSB (A15) bit of CPU addrress bus is not directly connected to A15 pin of ROM IC, but rather drives ROM IC's CE input pin (Chip Enable), and ROM IC's pin A15 is independently driven by P63 and RD outputs of CPU. In parallel to ROM IC A13, A14 and A15 outputs of CPU are connected to address decoder IC (SN74HC138N) with the following address map:

|Address range|Component|
|----------|------|
|0x2000 - 0x3FFF|OPZ|
|0x4000 - 0x5FFF|LCD|
|0x6000 - 0x7FFF|RAM|

So, when CPU P63 is HIGH (and it is set to HIGH during bootup), any address in range 0x8000 - 0xFFFF will go directly to ROM because A15 is set to HIGH thus enabling ROM IC during reads (RD is active). In case P63 is high but address on a bus is 0x7FFF or below, the A15 will go LOW thus disabling CE on ROM IC and address decoder will do it's work enabling OPZ or LCD or RAM depending on address range. However if address is 0x8000 or higher *and* P63 is low the ROM IC will be active but the reads will be performed from lower half of ROM IC - because ROM IC's A15 input is connected to CPU's P63 (which is LOW). Fortunately there's not a lot of code in lower half of ROM as disassembling it was a bit tricky.

TX81Z has several memory buffers of interest: 
* VMEM and AMEM are used to store patch (i.e. instrument) data as it is loaded from media (ROM or tape). VMEM data is common to DX100 and similar synths using YM2164 (aka OPP, the derivative of YM2151/OPM), AMEM holds parameters unique to OPZ.
* VCED and ACED are used to store patch data in "uncompressed" format suitable for editing. Also all values written to OPZ from VCED/ACED and not from VMEM/AMEM. VCED has ability to switch individual operator on/off but this is not stored in VMEM.
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
|`0x00`-`0x07`|VOL|VOL|VOL|VOL|VOL|VOL|VOL|VOL|Channel 0-7 volume|Unclear and unused|
|`0x08`|-|OP|OP|OP|OP|CH|CH|CH|Key ON/OFF|4 higher bits are operators, 3 lower - channel number. Key On event in OPZ is triggered when any OP for specific channel changes it's value from 0 to 1, Key Off triggered when operator changes value from 1 to 0. Normally all operators are running and there is no ability to switch operators on/off in VMEM/AMEM, however VCED has parameter 93 that allows to turn on/off specific operator but it is not documented and no menu item is present in TX81Z to change this setting. In other words, to set Key On for a channel one needs to write `0x78` to OP-part of this register, to set Key Off write `0x00`|
|`0x09`|||||||||Unknown|Set to `0x00` upon startup|
|`0x0A`|||||||||Unknown|Set to `0x04` upon startup|
|`0x0B`-`0x0E`|||||||||Unknown|Not referenced|
|`0x0F`|NE|-|-|-|NFRQ|NFRQ|NFRQ|NFRQ|Noise enable + noise frequency|OPM artifact. Not referenced|
|`0x10`|||||||||Timer A related?|Set to `0x00` upon startup|
|`0x11`-`0x13`|||||||||Unknown|Not referenced|
|`0x14`|||||||||Timer control|Set to `0x70` upon startup, DX100 does the same|
|`0x15`|||||||||Timer control|Set to `0x01` upon startup, DX100 does the same|
|`0x16`|LFRQ2|LFRQ2|LFRQ2|LFRQ2|LFRQ2|LFRQ2|LFRQ2|LFRQ2|LFO#2 frequency|TX81Z manual refers to this as 'LFO Speed', however 'frequency' appears to be more adequate term. TX81Z only uses LFO#2 in performance mode. This allows to have 2 instruments in one performance with independent LFOs. Value for this register depends on LFO waveform used - in case this is Noise/S&H waveform, a simple exponential calculation is performed, in other cases a lookup table is used that uses exponential function output as an index for selecting values from lookup table|
|`0x17`|`0` for AMD2 <br>`1` for PMD2|xMD2|xMD2|xMD2|xMD2|xMD2|xMD2|xMD2|AMD2 or PMD2|Actual amplitude modulation depth and phase modulation depth for LFO#2. Used when LFO#2 is used. Derived from VCED AMD and PMD. Values are calculated from basic AMD/PMD values present in VCED, MIDI controller values (Modulation Wheel, Breath and Foot Controllers), specific sensitivity to these CC values and LFO delay value. Detailed math explained in LFO section. LFO delay only affects *basic* AMD/PMD, other MIDI controllers affect AMD/PMD instantly. Highest bit indicates whether AMD2 or PMD2 is written in a register.|
|`0x18`|LFRQ1|LFRQ1|LFRQ1|LFRQ1|LFRQ1|LFRQ1|LFRQ1|LFRQ1|LFO#1 frequency|Normally used LFO. See also register `0x16` comments|
|`0x19`|`0` for AMD1 <br>`1` for PMD1|xMD1|xMD1|xMD1|xMD1|xMD1|xMD1|xMD1|AMD1 or PMD1|Used with LFO1. See also register `0x17` comments|
|`0x1A`|||||||||Unknown|Not referenced|
|`0x1B`|CT|CT|SY2|SY1|LW2|LW2|LW1|LW1|Control Terminal, LFO#2 Sync, LFO#1 Sync, LFO#2 Waveform, LFO#1 Waveform|CT is not used. LFO Sync (1-bit value for each LFO) means 'restart LFO on Key On event'. LFO Waveform is 2-bit value, 0x00 is saw-up, 0x01 is square, 0x02 is triangle, 0x03 is sample&hold i.e. noise|
|`0x1C`-`0x1F`|||||||||Unknown|Not referenced|
|`0x20`-`0x27`|R|?|FBL|FBL|FBL|ALG|ALG|ALG|Right output enable, Unknown, Feedback Level, Algorithm - for channels 0 to 7|OPM uses R and L (unknown here) for enabling sound output on right and left outputs. OPZ is different, logic is unclear so far. During Key On one needs to set R to 1 and Unknown to 0, and reverse values upon Key Off. Feedback Level is actually Operator #1 self-feedback, VCED parameter #53 as-is. Algorithm is VCED parameter #52 as-is|
|`0x28`-`0x2F`|-|KC|KC|KC|KC|KC|KC|KC|Key Code - for channels 0 to 7|As OPM does, OPZ also represents each note as `Octave number + Note in octave number`. There are 8 octaves each having 12 notes from C# to C. Note numbers in octave are `0, 1, 2, 4, 5, 6, 8, 9, 10, 12, 13, 14`, octave numbers are `0 to 7`. Octave `#4` and note `#10` is MIDI note `69` (A4 440 Hz).|
|`0x30`-`0x37`|KF|KF|KF|KF|KF|KF|-|MONO|Key Fraction and Mono flag - for channels 0 to 7|Each Key has 64 Fractions (6-bit value), mostly used when microtuning is in place. Mono flag is always set to `1`, need to investigate this deeper along with R and L outputs|
|`0x38`-`0x3F`|`0` for PMS1 <br>`1` for PMS2|PMS1 / PMS2|PMS1 / PMS2|PMS1 / PMS2|-|`0` for AMS1 <br>`1` for AMS2|AMS1 / AMS2|AMS1 / AMS2|Selector, PMS1 or PMS2, Selector, AMS1 or AMS2 - for channels 0 to 7|Selector is used to set PMS2 or PMS1 and AMS2 or AMS1. Selector value `0` is for PMS1 and AMS1, selector value `1` is for PMS2 and AMS2. PMS and AMS determine how sensitive the sound is to values in PMD and AMD. In case PMS or AMS will have value of `0` any value for PMD or AMD will not affect sound. PMS is VCED parameter #60 as-is, AMS is VCED paramerer #61 as-is|
|`0x40`-`0x5F`|`0` for DT1/FXRG and MUL/FXFREQ<br>`1` for OW and FINE|DT1 / FXRG / OW|DT1 / FXRG / OW|DT1 / FXRG / OW|MUL / FXFREQ / FINE|MUL / FXFREQ / FINE|MUL / FXFREQ / FINE|MUL / FXFREQ / FINE|Selector, Detune 1 or Fixed Range or Operator Waveform, Multiply or Fixed Frequency or Fine Frequency - for operators 4-2-3-1, for channels 0 to 7|OPZ has 2 modes for operator frequency - Ratio (same to OPM) and Fixed. If ACED parameter #0 is `0` then Ratio mode is used, if it is `1` - Fixed mode is used. In Ratio mode Detune 1 and Multiply values are used, in Fixed mode - Fixed Range and Fixed Frequency. In both modes the Fine Frequency is available. In Ratio mode Detune 1 is calculated from VCED parameter #12, and Multiply is calculated from VCED parameter #11 using lookup table. In Fixed mode the Fixed Range is ACED parameter #1 as-is, the Fixed Frequency is 4 upper bits of VCED parameter #11. In both modes ACED parameter #2 is used to set Fine Frequency tuning. When selector has value of `1` one can write Operator Waveform (ACED parameter #3 as-is) and Frequency Fine (ACED parameter #2 as-is)|
|`0x60`-`0x7F`|-|TL|TL|TL|TL|TL|TL|TL|Total Level - for operators 4-2-3-1, for channels 0 to 7|Most complex part of all calculations. `127` means zero volume. Detailed math explained separately|
|`0x80`-`0x9F`|KRS|KRS|FIX|AR|AR|AR|AR|AR|Key Rate Scaling, Fixed Flag and Attack Rate - for operators 4-2-3-1, for channels 0 to 7|Key Rate Scaling is VCED parameter #6 as-is. Fixed Flag is ACED parameter #0 as-is. Attack Rate is VCED parameter #0 as-is|
|`0xA0`-`0xBF`|AME|-|-|D1R|D1R|D1R|D1R|D1R|Amplitude Modulation Enable and Decay 1 Rate - for operators 4-2-3-1, for channels 0 to 7|Amplitude Modulation Enable is VCED parameter #8 as-is. Decay 1 Rate is VCED parameter #1 as-is|
|`0xC0`-`0xDF`|DT2 / EGBS|DT2 / EGBS|`0` for DT2/D2R <br>`1` for EGS/RR|D2R / -|D2R / -|D2R / REV|D2R / REV|D2R / REV|Detune 2 or EG Bias Shift, Selector, Decay 2 Rate or Reverb Rate - for operators 4-2-3-1, for channels 0 to 7|Detune 2 is calculated from VCED parameter #11 using lookup table. Decay 2 Rate is VCED parameter #2 as-is. EG Bias Shift is ACED parameter #4 as-is. Reverb Rate is ACED parameter #81 as-is (strange thing - this parameter is not operator-specific in ACED, but can be set independently for each operator in OPZ...)|
|`0xE0`-`0xFF`|D1L|D1L|D1L|D1L|RR|RR|RR|RR|Decay 1 Level and Release Rate - for operators 4-2-3-1, for channels 0 to 7|Decay 1 Level is (15 - VCED parameter #4). Release Rate is VCED parameter #3 as-is|
