# Program to calculate factorial using tiny8cpu

##Pseudocode without multiplication

//n = input
f = n
prevn = n - 2
while true
	if prevn=0 break
	subtotal = f
	//multiplication
	multi = prevn
	while true
		if multi == 0 break
		f = f + subtotal
		multi = multi - 1
	endwhile
	prevn = prevn -1
endwhile

##A program in tiny8cpu assembly language

```
LOADIA -1
STOREA $MINUSONE
LOADIA 5
STOREA $F
LOADB $MINUSONE
ADD
ADD
STOREA $PREVN
_LOOP_1_BEGIN:
LOADA $PREVN
JZ _LOOP_1_END
LOADA $F
STOREA $SUBTOTAL
LOADA $PREVN
STOREA $MULTI
_LOOP_2_BEGIN:
LOADA $MULTI
JZ _LOOP_2_END
LOADA $F
LOADB $SUBTOTAL
ADD
STOREA $F
LOADA $MULTI
LOADB $MINUSONE
ADD
STOREA $MULTI
LOADIA 0
JZ _LOOP_2_BEGIN
_LOOP_2_END:
LOADA $PREVN
LOADB $MINUSONE
ADD
STOREA $PREVN
LOADIA 0
JZ _LOOP_1_BEGIN
_LOOP_1_END:
LOADA $F
NOOP
```

## RAM Map
```
$MINUSONE  = 0x0 (address 0)
$F         = 0x1 (address 1) 
$PREVN     = 0x2 (address 2)
$SUBTOTAL  = 0x3 (address 3)
$MULTI     = 0x4 (address 4)
```

## Label Addresses
```
_LOOP_1_BEGIN = 0x08 (instruction 8)
_LOOP_1_END   = 0x20 (instruction 32)
_LOOP_2_BEGIN = 0x0E (instruction 14)
_LOOP_2_END   = 0x1A (instruction 26)
```

## Program with Replaced Labels and RAM Cells
```
0x00: LOADIA -1           ; Load immediate -1 (0xF in 4-bit two's complement)
0x01: STOREA 0x0          ; Store to $MINUSONE (address 0)
0x02: LOADIA 3            ; Load immediate 3
0x03: STOREA 0x1          ; Store to $F (address 1)
0x04: LOADB 0x0           ; Load $MINUSONE (address 0) into B
0x05: ADD                 ; Add A + B
0x06: ADD                 ; Add result again
0x07: STOREA 0x2          ; Store to $PREVN (address 2)
0x08: LOADA 0x2           ; _LOOP_1_BEGIN: Load $PREVN (address 2)
0x09: JZ 0x20             ; Jump to _LOOP_1_END if zero
0x0A: LOADA 0x1           ; Load $F (address 1)
0x0B: STOREA 0x3          ; Store to $SUBTOTAL (address 3)
0x0C: LOADA 0x2           ; Load $PREVN (address 2)
0x0D: STOREA 0x4          ; Store to $MULTI (address 4)
0x0E: LOADA 0x4           ; _LOOP_2_BEGIN: Load $MULTI (address 4)
0x0F: JZ 0x1A             ; Jump to _LOOP_2_END if zero
0x10: LOADA 0x1           ; Load $F (address 1)
0x11: LOADB 0x3           ; Load $SUBTOTAL (address 3) into B
0x12: ADD                 ; Add A + B
0x13: STOREA 0x1          ; Store to $F (address 1)
0x14: LOADA 0x4           ; Load $MULTI (address 4)
0x15: LOADB 0x0           ; Load $MINUSONE (address 0) into B
0x16: ADD                 ; Add A + B
0x17: STOREA 0x4          ; Store to $MULTI (address 4)
0x18: LOADIA 0            ; Load immediate 0
0x19: JZ 0x0E             ; Jump to _LOOP_2_BEGIN
0x1A: LOADA 0x2           ; _LOOP_2_END: Load $PREVN (address 2)
0x1B: LOADB 0x0           ; Load $MINUSONE (address 0) into B
0x1C: ADD                 ; Add A + B
0x1D: STOREA 0x2          ; Store to $PREVN (address 2)
0x1E: LOADIA 0            ; Load immediate 0
0x1F: JZ 0x08             ; Jump to _LOOP_1_BEGIN
0x20: LOADA 0x1           ; _LOOP_1_END: Load $F (address 1)
0x21: NOOP                ; No operation
```

## Translated Program in HEX
```
1F 40 13 41 30 55 55 42
22 A0 21 43 22 44 24 9A
21 33 55 41 24 30 55 44
10 8E 22 30 55 42 10 88
21 00
```
