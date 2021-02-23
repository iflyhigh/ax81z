# VCED and ACED buffers of Yamaha TX81Z

## VCED buffer

VCED consists of 2 parts: operator-specific parameters and global ones. Each operator has 13 specific parameters. They repeat in the same order for each operator. Operator order in VCED is `4-3-2-1`, while in VMEM and OPZ operators are ordered `4-2-3-1`. After 52-byte operator-specific part there is a section of global parameters. Parameters `#87` to `#92` are not actually present in TX81Z VCED memory structure so parameter `#93` (Operator enable flag) is actually parameter `#87`.

|Number|Number hex|Short name|Name|Data range|Comment|
|---|---|---|---|---|---|
||||||Operator 4|
|0|0x00|AR|Attack Rate|0-31|Envelope generator attack rate. Value of AR goes as-is to OPZ register `0x80`, 5 lower bits|
|1|0x01|D1R|Decay 1 Rate|0-31|Envelope generator decay 1 rate. Value of D1R goes as-is to OPZ register `0xA0`, 5 lower bits|
|2|0x02|D2R|Decay 2 Rate|0-31|Envelope generator decay 2 rate. Value of D2R goes as-is to OPZ register `0xC0`, 5 lower bits when selector bit #6 has value of `0`|
|3|0x03|RR|Release Rate|0-15|Envelope generator release rate. Value of RR goes as-is to OPZ register `0xE0`, 4 lower bits|
|4|0x04|D1L|Decay 1 Level|0-15|Envelope generator decay 1 level. Value of (15-D1L) goes as-is to OPZ register `0xE0`, 4 upper bits|
|5|0x05|KLS|Key Level Scaling|0-99|Decreases volume of higher notes. The higher the setting, the quiter the higher notes will be. Setting affects Total Level OPZ register `0x60`, math is explained separately|
|6|0x06|KRS|Key Rate Scaling|0-3|Increases envelope speed of higher notes. The higher the setting, the faster the higher notes will be. Value of KRS goes as-is to OPZ register `0x80`, 3 upper bits|
|7|0x07|EBS|EG Bias Sensitivity|0-7|??? Setting affects Total Level OPZ register `0x60`, math is explained separately|
|8|0x08|AME|Amplitude Modulation Enable|0-1|Make operator's amplitude sensitive to LFO. Operator's pitch is always sensitive to LFO. Value of AME goes as-is to OPZ register `0xA0` bit 7|
|9|0x09|KVS|Key Velocity Sensitivity|0-7|Sets operator's sensitivity to MIDI velocity. Setting affects Total Level OPZ register `0x60`, math is explained separately|
|10|0x0A|OUT|Output level|0-99|Operator's basic (initial) output level. Setting affects Total Level OPZ register `0x60`, math is explained separately|
|11|0x0B|FREQ|Frequency|0-63|Operator's basic frequency. In Ratio mode this value affects Multiply (OPZ register `0x40` 4 lower bits) using lookup table, and Detune 2 (OPZ register `0xC0` 2 upper bits when selector bit 6 is set to `0`) also using lookup table. In Fixed mode 4 upper bits of this value is Fixed Frequency and go as-is to OPZ register `0x40` lower 4 bits, and Detune 2 (OPZ register `0xC0` 2 upper bits when selector bit 6 is set to `0`) is calculated using lookup table in the same way as in Ratio mode.|
|12|0x0B|DET|Detune|0-6|Center value is 3. Value of DET is used only in Ratio mode, it is processed with the following logic `(DET<3)?(7-DET):(DET-3)` and goes to OPZ register `0x40`, 3 upper bits when selector bit 7 is set to `0`|
||||||Operator 3|
|13|0x0C||||...|
|...|...|...|...|...|...|
|25|0x19||||...|
||||||Operator 2|
|26|0x1A||||...|
|...|...|...|...|...|...|
|38|0x26||||...|
||||||Operator 1|
|39|0x27||||...|
|...|...|...|...|...|...|
|51|0x33||||...|
|52|0x34|ALG|Algorithm|0-7|Algorithm of operator connections. Value of ALG goes as-is to OPZ register `0x20`, 3 lower bits. Also affects Total Level OPZ register `0x60`, math is explained separately|
|53|0x35|FBL|Feedback Level|0-7|Self-feedback level for operator 4. Value of FBL goes as-is to OPZ register `0x20`, bits 3-5|
|54|0x36|LFOFREQ|LFO Frequency|0-99|LFO frequency. Manual calls it 'LFO Speed'. Value is processed using exponential function in case of sample&hold LFO waveform or lookup table in other cases and goes to OPZ register `0x16` (LFO#2) or `0x18` (LFO#1)|
|55|0x37|LFODEL|LFO Delay|0-99|Non-zero value will cause basic (initial) AMD and PMD to start growing from zero up to desired value. MIDI CC changes are not affected by LFO delay, i.e. rotating modulation wheel with non-zero sensitivity (VCED parameters `#60` and `#61` for modulation wheel) will cause LFO applied (AMD and PMD values written to OPZ register `0x17` or `0x19`) immediately proportional to modulation wheel position. LFO delay itself is independent from MIDI CCs and will grow up to desired value once the key is still pressed. Math is explained separately|
|56|0x38|PMD|Pitch Modulation Depth|0-99|Basic (initial) amount of LFO modulation applied to all operators' pitch. Value is processed using exponential function, multiplied by current LFO delay effect (255 in case of no/expired delay). Then modulation wheel, breath controller and foot controller adjustments are applied and added to basic PMD. Finally the value is divied by 2 and goes to OPZ register `0x17` or `0x19` with bit 7 set to `1`. Pitch Modulation Sensitivity (VCED parameter `#60`) does no affect this value|
|57|0x39|AMD|Amplitude Modulation Depth|0-99|Basic (initial) amount of LFO modulation applied to all operators' amplitude. Value is processed using exponential function, multiplied by current LFO delay effect (255 in case of no/expired delay). Then modulation wheel, breath controller and foot controller adjustments are applied and added to basic AMD. Finally the value is substracted from `0xff` and result is used as an index in lookup table (same lookup table is used for non-noise LFO frequency calculations). Value from lookup table goes to OPZ register `0x17` or `0x19` with bit 7 set to `0`. Amplitude Modulation Sensitivity (VCED parameter `#61`) or Amplitude Modulation Enable (VCED parameter `#8`) do no affect this value|
|58|0x3A|LFOSY|LFO Sync|0-1|Value of `1` will cause LFO to be restarted upon Key On. Value goes as-is to OPZ register `0x1B` bits `4` or `5`|
|59|0x3B|LFOWF|LFO Waveform|0-3|Saw up, square, triangle or sample&hold (noise). Value goes as-is to OPZ register `0x1B` bits `0` and `1` or `2` and `3`|
|60|0x3C|PMS|Pitch Modulation Sensitivity|0-7|Determines all operators' pitch sensitivity to LFO modulation applied in PMD register. Value goes as-is to OPZ register `0x38` bits `4` to `6`|
|61|0x3D|AMS|Amplitude Modulation Sensitivity|0-3|Determines all operators' amplitude sensitivity to LFO modulation applied in AMD register. Operator with AME (VCED parameter `#8`) disabled will not be affected by any AMS value. Value goes as-is to OPZ register `0x38` bits `0` and`1`|
|62|0x3E|TPS|Transpose|0-48|Center value is `24`. Used to shift (transpose) by specified number of MIDI notes. Affects OPZ register `0x28` (Key Code)|
|63|0x3F|POLY|Poly Mode|0-1|In Mono mode synth will use a single channel. Affects portamento implementations|
|64|0x40|PBR|Pitch Bend Range|0-12|Sets the range the pitch wheel will affect note frequency, measured in number of notes. Value of `12` means whole octave up and whole octave down|
|65|0x41|PRTM|Portamento Mode|0-1|???|
|66|0x42|PRTT|Portamento Time|0-99|???|
|67|0x43|FCVOL|Foot Controller Volume|0-99|??? How foot controller affects the volume. Setting probably affects Total Level OPZ register `0x60` ???|
|68|0x44|SUST|Sustain|0-1|Unused|
|69|0x45|PORT|Portamento|0-1|Unused|
|70|0x46|CHOR|Chorus|0-1|Unused|
|71|0x47|MWPE|Modulation Wheel Pitch Effect|0-99|Determines amount added to basic (initial) PMD by modulation wheel. The value is processed with exponential function then multiplied by 2 then multiplied by MIDI CC#1 value (0-127). Result is added to basic PMD|
|72|0x48|MWAE|Modulation Wheel Amplitude Effect|0-99|Determines amount added to basic (initial) AMD by modulation wheel. The value is processed with exponential function then multiplied by 2 then multiplied by MIDI CC#1 value (0-127). Result is added to basic AMD|
|73|0x49|BCPE|Breath Controller Pitch Effect|0-99|Same as MWPE but for breath controller. MIDI CC#2|
|74|0x4A|BCAE|Breath Controller Amplitude Effect|0-99|Same as MWAE but for breath controller. MIDI CC#2|
|73|0x49|BCPB|Breath Controller Pitch Bias|0-99|Center value is `50` ???|
|74|0x4A|BCEGB|Breath Controller EG Bias|0-99|???|
|75|0x4B|NAM0|Voice name char 0|32-127||
|...|...|...|...|...||
|86|0x56|NAM9|Voice name char 9|32-127||
|87|0x57|OPE|Operator enable|0-15|4 bits, each enables (`1`) or disables (`0`) a single operator|

## ACED buffer

ACED buffer is located right after VCED in TX81Z memory so firmware uses this to have unified addressing.

|Number|Number hex|Short name|Name|Data range|Comment|
|---|---|---|---|---|---|
||||||Operator 4|
|0|0x00|AR|Attack Rate|0-31|Envelope generator attack rate. Value of AR goes as-is to OPZ register `0x80`, 5 lower bits|
||||||Operator 3|
|13|0x0C||||...|
|...|...|...|...|...|...|
|25|0x19||||...|
||||||Operator 2|
|26|0x1A||||...|
|...|...|...|...|...|...|
|38|0x26||||...|
||||||Operator 1|
|39|0x27||||...|
|...|...|...|...|...|...|
|51|0x33||||...|
|52|0x34|ALG|Algorithm|0-7|Algorithm of operator connections. Value of ALG goes as-is to OPZ register `0x20`, 3 lower bits. Also affects Total Level OPZ register `0x60`, math is explained separately|
