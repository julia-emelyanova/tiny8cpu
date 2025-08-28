# tiny8cpu ai prompts

# A prompt to translate Instruction Set opcodes to ROM hex table

```
Translate into control rom contents, where the first line is an address at which contents could be read, and the second line is contents. If the address is missing, its contents should be replaced with an appropriate amount of 0. In the end provide only contents as string in hex devided with spaces
```

# A prompt to translate a program into machine language

```
Translate the program into machine code using the opcode table. Each instruction consists of one byte: Opcode (4 bits), Operand (4 bits). Immediate values like -1 are in two's complement format with signed 4-bit interpretation.
Variables, starting with $ are 8-bit RAM cells with 4-bit addresses. Please replace them with appropriate addresses
Strings, starting with "_" and ending with ":" are labels. Strings, starting with "_" are addresses. Please replace addresses according to the position of the labels.

Provide
- RAM map
- a program with replaced labels and RAM cells
- translated program in HEX

Opcodes:

NOOP
0000
LOADIA
0001
LOADA
0010
LOADB
0011
STOREA
0100
ADD 
0101
XOR
0110
JZ
1000


Program:

LOADIA 1
LOADIA 2
LOADIA 3
LOADIA 4
JZ 2              ;won’t jump
LOADIA 0
JZ 2             ;should jump

```
