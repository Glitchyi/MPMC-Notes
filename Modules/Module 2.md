# Addressing modes
Addressing mode indicates the way in which the microprocessor locates data or operands. Instructions may be classified into two according to the flow of execution:
1) Sequential control flow
2) Control transfer 

## Addressing modes in 8086
### Immediate 
The data is part of the instruction, usually written as values in hexadecimal

```
MOV AX, 0005H
```
### Direct
16-bit memory address (offset) or an IO address is directly specifies in the instruction

```
MOV AX, [5000H]
```

Internally the address is:
$$
\text{Effective Address } = 10\text{H} \star DS + \langle\text{address}\rangle
$$
### Register
Registers in which data is stored is used, all register except the IP (Instruction Pointer) can be used in this mode

```
MOV AX, BX
```

### Indirect Register
Registers that contain the address (offset) pointing to the data are used, the data will be at the location where the value of the register points to, for example for the command ` MOV AX, [BX] ` and `BX` has the value `2000H`, then the data is at the offset`2000H`.

```
MOV AX, [BX]
```

Internally the address is:
$$
\text{Effective Address } = 10\text{H} \star DS + \langle \text{ address at register i.e. [register] }\rangle
$$
### Indexed
The offset of the operand is stored in one of the index registers. This is a special case mode of the Indirect Register addressing mode. By default the `SI` and `DI` registers use the `DS` segment. In case of strings `SI` handles `DS` and `DI` handles `ES`.

```
MOV AX, [SI]
```

Internally the address is:
$$
\text{Effective Address } = 10\text{H} \star DS + \langle \text{ [register] }\rangle
$$
### Register Relative 
In this addressing mode the address is stored in a register but an offset is also provided which will be used to find the effective address relative to the address in the register

```
MOV AX, 50H[BX]
```

Internally the address is:
$$
\text{Effective Address } = 10\text{H} \star DS + \langle \text{ offset  value }\rangle+\langle \text{ [register] }\rangle
$$
### Based Indexed
Using the base register `BX` or `BP` the effective address is calculated by adding the index registers `SI` or `DI`. The `BX` or `BP` registers are used to relatively find the effective address.

```
MOV AX, [BX][SI]
```

Internally the address is:
$$
\text{Effective Address } = 10\text{H} \star DS + \langle \text{ [register 1] }\rangle+\langle \text{ [register 2] }\rangle
$$
### Relative Based Indexed
This addressing mode is a combination of Register Relative as well as Based Index, by making use of both an offset value before the registers and then using a base register to find the offset from.

```
MOV AX, 50H [BX][SI]
```

Internally the address is:
$$
\text{Effective Address } = 10\text{H} \star DS +\langle \text{ offset value }\rangle+ \langle \text{ [register 1] }\rangle+\langle \text{ [register 2] }\rangle
$$
### Intrasegment Indirect Mode
In this mode the displacement to which the control is to be transferred is the same segment in which the controls transfer instruction lies.

```
JMP [BX]
```
> Jump to the effective address stored in BX.

### Intersegment direct
In this mode, the address to which the control is to be transferred lies in a different segment and it is passed to the instruction directly.

```
JMP 5000H : 2000H
```
> Jump to the effective address `2000H` in segment `5000H`

### Intersegment indirect
In this mode, the address to which the control is to be transferred lies in a different segment and it is passed to the instruction indirectly.

```
JMP [2000H]
```
> Jump to the effective address specified at `2000H` in DS.

# Instruction Set
Instruction set refers to the commands the microcontroller uses to perform certain actions.

## Instruction set for 8086

### Data Copy/Transfer Instructions

**MOV** : The instruction transfers data from the source address/registers to the destination address/registers.

**PUSH** : This instruction pushed the contents of the specified registers or memory address onto the stack, the stack pointer is always decremented by 2 after this operation.
![[Images/Pasted image 20231219133009.png]]

**POP** : The POP instruction loads specified register/memory location with the contents from the top of the stack, the stack pointer is incremented by 2 after this operation

**XCHG** : The instruction swaps the data at the specified data or memory address.

**IN** : This instruction is used read an input port, `AL` and `AX` registers are made use to read input from the ports, The port address for the input port is to be stored at the `DX` register.

**OUT** : Similar to the `IN` Instruction this instruction allows the microprocess to output data to a specified port, using the `AX` or `AL`  registers. Again the port address is stored in the `DX` register, the data to an odd address port is transferred on $D_8-D_{15}$ , while for an even addressed  port the addresses $D_0-D_7$ are used

> For both `IN` and `OUT` instructions 8-bit port address can be specified directly, but 16-bit addresses should be in loaded onto the `DX` register.

**XLAT** : This instruction is used to find out codes in case of code conversion.

**LEA** : The instruction is used to load the effective address into a register

## Arithmetic Instructions

**ADD** : This instruction adds the contents of source operand with the contents of destination operand. The result is stored in destination operand.

**ADC** : This instruction adds the contents of source operand with the contents of destination operand with carry flag bit.

**SUB** : This instruction subtracts the source operand from the destination operand. The result is stored in destination operand. 

**SBB** : This instruction subtracts the source operand and borrow flag(BF) from the destination operand. The result is stored in destination operand.

**INC** : This instruction increases the contents of source operand by 1.

