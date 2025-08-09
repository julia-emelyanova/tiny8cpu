# Program to add 2 numbers using tiny8cpu

## Original program 
```
LOADIA, 3        
STOREA, 10      
LOADB, 10        
LOADIA, 2        
ADD
```

## Opcode Mapping
Based on opcode table, instructions will be mapped as follows:
- `LOADIA, 3` → `LOADIA 3` (0001)
- `STOREA, 10` → `STOREA 10` (0100)  
- `LOADB, 10` → `LOADB 10` (0011)
- `LOADIA, 2` → `LOADIA 2` (0001)
- `ADD` → `ADD 0` (0101) - operand 0 for register operation


## Machine Code Translation

| Address | Instruction | Opcode | Operand | Machine Code | Comments |
|---------|-------------|---------|----------|--------------|----------|
| 0 | LOADIA 3 | 0001 | 0011 | 0x13 | Load immediate 3 into A |
| 1 | STOREA 10 | 0100 | 1010 | 0x4A | Store A into address 10 |
| 2 | LOADB 10 | 0011 | 1010 | 0x3A | Load from address 10 into B |
| 3 | LOADIA 2 | 0001 | 0010 | 0x12 | Load immediate 2 into A |
| 4 | ADD 0 | 0101 | 0000 | 0x50 | Add A and B |

## Final Machine Code (HEX)
```
13 4A 3A 12 50
```