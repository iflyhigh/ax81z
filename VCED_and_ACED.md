# VCED and ACED buffers of Yamaha TX81Z

## VCED buffer

VCED consists of 2 parts: operator-specific parameters and global ones. Each operator has 13 specific parameters. They repeat in the same order for each operator. Operator order in VCED is `4-3-2-1`, while in VMEM and OPZ operators are ordered `4-2-3-1`. After 52-byte operator-specific part there is a section of global parameters.

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
|53|0x35|FBL|Feedback Level|0-7|Self-feedback level for operator 1. Value of FBL goes as-is to OPZ register `0x20`, bits 3-5|
|54|0x36|LFOFREQ|LFO Frequency|0-99|LFO frequency. Manual calls it 'LFO Speed'. Value is processed using exponential function in case of sample&hold LFO waveform or lookup table in other cases and goes to OPZ register `0x16` (LFO#2) or `0x18` (LFO#1)|
|55|0x37|LFODEL|LFO Delay|0-99|Non-zero value will cause basic (initial) AMD and PMD to start growing from zero up to desired value. MIDI CC changes are not affected by LFO delay, i.e. rotating modulation wheel with non-zero sensitivity (VCED parameters `#60` and `#61` for modulation wheel) will cause LFO applied (AMD and PMD values written to OPZ register `0x17` or `0x19`) immediately proportional to modulation wheel position. LFO delay itself is independent from MIDI CCs and will grow up to desired value once the key is still pressed. Math is explained separately|
