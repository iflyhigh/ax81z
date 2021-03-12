## TX81Z math

### Exponential function

A lot of VCED parameters that take values from `0` to `99` are processed using exponential function ('expo' for later use). TX81Z does not actually calculate the real exponent but rather uses approximation which is good at `[0..99]` range. The following M6800 assembly code is used to calculate:

```
ROM:908B                 ldab    #$A5
ROM:908D                 mul
ROM:908E                 lsld
ROM:908F                 lsld
```
This is equivalent to the following code (in Java for some reason)

```
	private static int f_expo(int in) {
		return 0xff & ((in * 165) >> 6);
	}
```
