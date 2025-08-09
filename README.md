# tiny8cpu

## 1. Overview

**tiny8cpu** is a minimalist 8-bit CPU implemented in Logisim Evolution, designed as an educational project to demonstrate CPU architecture, ALU design, and control signal decoding.

**Key features:**

* Minimal instruction set and control bus
* 8-bit architecture
* Variable-length instructions

---

## 2. Architecture

* **Registers:** A, B, PC (program counter)
* **Program Memory:** 7-bit address space ROM
* **RAM:** 4-bit address space 
* **ALU:** supports addition and XOR
* **Control Unit:** decodes opcodes into control signals

---

### 3. ISA 

Instruction length is 8 bit. 

| Instruction | Opcode     | Control bits | Description                          |
| ----------- | ---------- | ------------ | ------------------------------------ |
| **NOOP**    | `0000`     | `00000000`   | No operation                         |
| **LOADIA**  | `0001`     | `10000011`   | Load immediate value into register A. Immediate operands are 4-bit signed values. |
| **LOADA**   | `0010`     | `10010001`   | Load from memory into register A     |
| **LOADB**   | `0011`     | `01010001`   | Load from memory into register B     |
| **STOREA**  | `0100`     | `00001001`   | Store register A into memory         |
| **ADD**     | `0101`     | `10000101`   | Add A and B, store result in A       |
| **XOR**     | `0110`     | `10100101`   | Bitwise XOR A and B, store in A      |
| **JZ**      | `1` | `not specified`     | Jump if A == 0 to 7-bit address      |

---

### 4. Control Bus

The **control bus** is an 8-bit signal line that coordinates all major CPU operations.
Each bit corresponds to a specific control signal. When set to `1`, the signal is active; when set to `0`, it is inactive.

**Bit layout (MSB → LSB):**

```
[LoadA][LoadB][ALUOP][MemRead][MemWrite][UseAluRes][UseImmediate][PC_inc]
```

| Bit | Name             | Function                                                                                     |
| --- | ---------------- | -------------------------------------------------------------------------------------------- |
| 7   | **LoadA**        | Loads data into register A from the selected data source (memory, immediate, or ALU result). |
| 6   | **LoadB**        | Loads data into register B from memory.                                                      |
| 5   | **ALUOP**        | Selects the ALU operation (0 = ADD, 1 = XOR).                                                |
| 4   | **MemRead**      | Enables reading data from memory into the data bus.                                          |
| 3   | **MemWrite**     | Enables writing data from the data bus into memory.                                          |
| 2   | **UseAluRes**    | Routes the ALU result to the selected register.                                              |
| 1   | **UseImmediate** | Routes an immediate value (from instruction) instead of memory data.                         |
| 0   | **PC\_inc**      | Increments the Program Counter (PC) to point to the next instruction.                        |

---

### 5. Example Program

```
LOADIA 5
STOREA 10
LOADIA 3
ADD
STOREA 11
```

Result: `memory[10] = 5`, `memory[11] = 8`.