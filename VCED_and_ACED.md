# VCED and ACED buffers of Yamaha TX81Z

## VCED buffer

VCED consists of 2 parts: operator-specific parameters and global ones. Each operator has 13 specific parameters. They repeat in the same order for each operator. Operator order in VCED is `4-3-2-1`, while in VMEM and OPZ operators are ordered `4-2-3-1`.

|Number|Number hex|Short name|Name|Data range|Comment|
|---|---|---|---|---|
|0|0x00|AR|Attack Rate|0-31|Operator 4 envelope generator attack rate|
