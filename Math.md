## TX81Z math

### Exponential function

A lot of VCED parameters that take values from `0` to `99` are processed using exponential function (`expo()` for later use). TX81Z does not actually calculate the real exponent but rather uses approximation which is good at `[0..99]` range. The following M6800 assembly code is used to calculate this function:

```
ROM:908B                 ldab    #$A5
ROM:908D                 mul
ROM:908E                 lsld
ROM:908F                 lsld
ROM:9090                 rts
```
Assuming the VCED value is in `ACCA` register and result is also read from `ACCA` register it is equivalent to the following code:

```
int expo(int in) {
    return 0xff & ((in * 165) >> 6);
}
```

### LFO

LFO frequency (or speed as manual suggests) is calculated dependent on LFO waveform. For waveform `3` (Noise/Sample&Hold) the `expo()` function is used. For other waveforms the 256-byte lookup table accessed by `expo()` value of VCED parameter is used (i.e. `lfo_frequency = c_table_LFO[expo(VCED_LFO_freq)]`). 

```
int c_table_LFO[] =
{
	255, 255, 224, 205, 192, 181, 173, 166, 
	160, 154, 149, 145, 141, 137, 134, 130, 
	128, 125, 122, 120, 117, 115, 113, 111, 
	109, 107, 105, 103, 102, 100, 98,  97,  
	96,  94,  93,  91,  90,  89,  88,  86,  
	85,  84,  83,  82,  81,  80,  79,  78,  
	77,  76,  75,  74,  73,  72,  71,  70,  
	70,  69,  68,  67,  66,  66,  65,  64,  
	64,  63,  62,  61,  61,  60,  59,  59,  
	58,  57,  57,  56,  56,  55,  54,  54,  
	53,  53,  52,  51,  51,  50,  50,  49,  
	49,  48,  48,  47,  47,  46,  46,  45,  
	45,  44,  44,  43,  43,  42,  42,  42,  
	41,  41,  40,  40,  39,  39,  38,  38,  
	38,  37,  37,  36,  36,  36,  35,  35,  
	34,  34,  34,  33,  33,  33,  32,  32,  
	32,  31,  31,  30,  30,  30,  29,  29,  
	29,  28,  28,  28,  27,  27,  27,  26,  
	26,  26,  25,  25,  25,  24,  24,  24,  
	24,  23,  23,  23,  22,  22,  22,  21,  
	21,  21,  21,  20,  20,  20,  19,  19,  
	19,  19,  18,  18,  18,  18,  17,  17,  
	17,  17,  16,  16,  16,  16,  15,  15,  
	15,  15,  14,  14,  14,  14,  13,  13,  
	13,  13,  12,  12,  12,  12,  11,  11,  
	11,  11,  10,  10,  10,  10,  9,   9,   
	9,   9,   8,   8,   8,   8,   8,   7,   
	7,   7,   7,   6,   6,   6,   6,   6,   
	5,   5,   5,   5,   5,   4,   4,   4,   
	4,   4,   3,   3,   3,   3,   3,   2,   
	2,   2,   2,   2,   2,   1,   1,   1,   
	1,   1,   0,   0,   0,   0,   0,   0
};
```

### Key code
OPZ internally supports 96 notes, 12 notes per octave, 8 octaves total. Recognized MIDI note numbers are `13` to `108`, the following lookup table is used to match MIDI note numbers to OPZ note numbers. I.e., `opz_note_number = c_table_KEYCODE[(midi_note_number - 13)]`.
```
int c_table_KEYCODE[] =
{
	0,   1,   2,   4,   5,   6,   8,   9,   
	10,  12,  13,  14,  16,  17,  18,  20,  
	21,  22,  24,  25,  26,  28,  29,  30,  
	32,  33,  34,  36,  37,  38,  40,  41,  
	42,  44,  45,  46,  48,  49,  50,  52,  
	53,  54,  56,  57,  58,  60,  61,  62,  
	64,  65,  66,  68,  69,  70,  72,  73,  
	74,  76,  77,  78,  80,  81,  82,  84,  
	85,  86,  88,  89,  90,  92,  93,  94,  
	96,  97,  98,  100, 101, 102, 104, 105, 
	106, 108, 109, 110, 112, 113, 114, 116, 
	117, 118, 120, 121, 122, 124, 125, 126
};
```
Also OPM documentation uncovers the following details that appear to be similar with OPZ: register `0x28` has 3 higher bits for octave number and 4 lower bits for note number in octave. Note numbers in octave are `0, 1, 2, 4, 5, 6, 8, 9, 10, 12, 13, 14`, octave numbers are `0` to `7`. So some simple math can be used for note number calculation, although TX81Z uses just the lookup table.

### PMD
Both PMD1 and PMD2 are processed in the same way. "Basic" value found in VCED parameter `#56` is processed by `expo()` function then multiplied by current LFO delay effect value (`255` if no LFO delay or delay has expired), then 8 higher bits are used for further calculations. Then processed Modulation Wheel Pitch Effect (VCED `#71`), Breath Controller Pitch Effect (VCED `#73`) and Foot Controller Pitch Effect (ACED `#21`) are added to previously processed PMD value. Each 'XXX Pitch Effect' is processed using the following formula:
```
expo(XXX_pitch_effect) * XXX_midi_cc_value * 2
```
, then 8 higher bits are added to processed PMD value. If the resulting value exceeds `255` the value of `255` is used further. 7 higher bits of resulting value are used as actual value written to OPZ register `0x17` or `0x19` with highest bit set to `1`. A pseudocode for this logic is as follows:
```
int basic_part = ((expo(basic_pmd) * lfo_delay_effect) >> 8);
int mw_effect_part = ((expo(pmd_mw_sensitivity) * mw_cc_value * 2) >> 8);
int bc_effect_part = ((expo(pmd_bc_sensitivity) * bc_cc_value * 2) >> 8);
int fc_effect_part = ((expo(pmd_fc_sensitivity) * fc_cc_value * 2) >> 8);
int pmd = basic_part + mw_effect_part + bc_effect_part + fc_effect_part;
if (pmd > 255) { pmd = 127; }
else { pmd = pmd >> 1; }
```
### AMD
Both AMD1 and AMD2 are processed in the same way. "Basic" value found in VCED parameter `#57` is processed by `expo()` function then multiplied by current LFO delay effect value (`255` if no LFO delay or delay has expired), then 8 higher bits are used for further calculations. Then processed Modulation Wheel Amplitude Effect (VCED `#72`), Breath Controller Amplitude Effect (VCED `#74`) and Foot Controller Amplitude Effect (ACED `#22`) are added to previously processed AMD value. Each 'XXX Amplitude Effect' is calculated using the following formula:
```
expo(XXX_amplitude_effect) * XXX_midi_cc_value * 2
```
, then 8 higher bits are added to processed AMD value. If the resulting value exceeds `255` the value of `255` is used further. Next, the value is substracted from `0xff` and result is used as an index in `c_table_LFO` table. Finally, 7 higher bits are used as actual value written to OPZ register `0x17` or `0x19` with highest bit set to `0`. A pseudocode for this logic is as follows:
```
int basic_part = ((expo(basic_amd) * lfo_delay_effect) >> 8);
int mw_effect_part = ((expo(amd_mw_sensitivity) * mw_cc_value * 2) >> 8);
int bc_effect_part = ((expo(amd_bc_sensitivity) * bc_cc_value * 2) >> 8);
int fc_effect_part = ((expo(amd_fc_sensitivity) * fc_cc_value * 2) >> 8);
int c_lfo_table_index = 0xff - (basic_part + mw_effect_part + bc_effect_part + fc_effect_part);
if (c_lfo_table_index < 0) { c_lfo_table_index = 0; }		
int amd = c_table_LFO[c_lfo_table_index] >> 1;
```
### KLS
```
int c_table_KLS[] =
{
	0,   1,   2,   3,   4,   5,   6,   7,   8,   9,   
	11,  14,  16,  19,  23,  28,  33,  39,  47,  57,  
	67,  80,  95,  113, 134, 160, 190, 224, 255
};
```
### KVS
```
int kvs = 1; // possible values 0..7
int kvsLow = 0xFF - (0xF0 | (kvs << 1));
int kvsHigh = 0xFF & (0x20 * kvs);
```
### Pitch wheel
First, a pitch bend range (VCED parameter `#64`) is converted using the following lookup table:
```
int c_table_PITCH_BEND_RANGE[] =
{
	16, 33, 49, 65, 81, 97, 113, 129, 145, 162, 178, 194   
}
```
### TL
```
int c_table_BASIC_TL[] =
{
	127, 122, 118, 114, 110, 107, 104, 102, 100, 98,  
	96,  94,  92,  90,  88,  86,  85,  84,  82,  81
};

int c_table_TL_ALG[][] =
{
	{0,   0,   0,   0},
	{0,   0,   0,   0},
	{0,   0,   0,   0},
	{0,   0,   0,   0},	
	{0,   0,   8,   8},
	{0,  13,  13,  13},
	{0,  13,  13,  13},
	{16, 16,  16,  16}
};
```
