# TX81Z analog sound part

## On OpAmps
Original TX81Z has been designed to use a transformer-based 10W power supply providing +5V, +15V and -15V voltages. While +5V is used to power all digital parts of the device, dual +/-15V voltage is used to provide Vcc+ and Vcc- to several operational amplifiers used in analog circuit. Nowdays a lot of single-supply opamps are available thus allowing to simplify power supply design. The following sigle-supply replacement opamps has been tested and found to provide good sound quality:

|Original position|Original OpAmp|Replacement OpAmp|
|----------|------|------|
|IC1|NJM4558DV|NE5532|
|IC2|NJM072D|MC34072 or AD8532|
|IC10|NJM072D|MC34072 or AD8532|
