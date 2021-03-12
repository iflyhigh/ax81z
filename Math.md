## TX81Z math

### Exponential function

A lot of VCED parameters that take values from `0` to `99` are processed using exponential function (`expo()` for later use). TX81Z does not actually calculate the real exponent but rather uses approximation which is good at `[0..99]` range. The following M6800 assembly code is used to calculate this function assuming the VCED value is in `ACCA` register and result is also read from `ACCA` register:

```
ROM:908B                 ldab    #$A5
ROM:908D                 mul
ROM:908E                 lsld
ROM:908F                 lsld
ROM:9090                 rts
```
This is equivalent to the following code:

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
				255, 255, 224, 205, 192, 181, 173, 166, 160, 154,
				149, 145, 141, 137, 134, 130, 128, 125, 122, 120,
				117, 115, 113, 111, 109, 107, 105, 103, 102, 100,
				98, 97, 96, 94, 93, 91, 90, 89, 88, 86,
				85, 84, 83, 82, 81, 80, 79, 78, 77, 76,
				75, 74, 73, 72, 71, 70, 70, 69, 68, 67,
				66, 66, 65, 64, 64, 63, 62, 61, 61, 60,
				59, 59, 58, 57, 57, 56, 56, 55, 54, 54,
				53, 53, 52, 51, 51, 50, 50, 49, 49, 48,
				48, 47, 47, 46, 46, 45, 45, 44, 44, 43,
				43, 42, 42, 42, 41, 41, 40, 40, 39, 39,
				38, 38, 38, 37, 37, 36, 36, 36, 35, 35,
				34, 34, 34, 33, 33, 33, 32, 32, 32, 31,
				31, 30, 30, 30, 29, 29, 29, 28, 28, 28,
				27, 27, 27, 26, 26, 26, 25, 25, 25, 24,
				24, 24, 24, 23, 23, 23, 22, 22, 22, 21,
				21, 21, 21, 20, 20, 20, 19, 19, 19, 19,
				18, 18, 18, 18, 17, 17, 17, 17, 16, 16,
				16, 16, 15, 15, 15, 15, 14, 14, 14, 14,
				13, 13, 13, 13, 12, 12, 12, 12, 11, 11,
				11, 11, 10, 10, 10, 10, 9, 9, 9, 9,
				8, 8, 8, 8, 8, 7, 7, 7, 7, 6,
				6, 6, 6, 6, 5, 5, 5, 5, 5, 4,
				4, 4, 4, 4, 3, 3, 3, 3, 3, 2,
				2, 2, 2, 2, 2, 1, 1, 1, 1, 1,
				0, 0, 0, 0, 0, 0
		};

```

### Key code

### PMD

### AMD

### KLS

### KVS

### TL
