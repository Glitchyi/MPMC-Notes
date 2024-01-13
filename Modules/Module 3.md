## Interrupts
The meaning of an interrupt tis to break the sequence of operation, i.e. break the normal sequence of execution of instructions, diverts its execution to some other program called `interrupt service routine (ISR)`. After the `ISR` is done execution it return the control to the main program

## Types of Interrupts
- Hardware interrupts/External Interrupts
- Software Interrupts/Internal interrupts

![[Images/Pasted image 20231226193414.png]]

### Hardware Interrupt
8086 has two types of hardware interrupts hardware and software.
#### Maskable Interrupts
Interrupts which can be either accepted or rejected by the processor are called maskable interrupts, in 8086 these types of hardware interrupts are initiated thorough the INTR pin, these are maskable by clearing the interrupt flag, the priority of which is determined by an external programable interrupt controller.

#### Non Maskable Interrupts
The interrupts which cannot be rejected are called as non maskable interrupts, whenever an NMI has be initiated, the processor suspends the current program and starts execution of `ISR` (`Interrupt Service Routine`). The non 

- Software interrupts in 8086 (dedicated, user-defined)
- Interrupt handling process in 8086
2. Interrupt Vector Table (IVT)
- Structure and purpose
- Mapping interrupt types to addresses
3. Nested interrupts
- Control transfer when interrupts occur during an ISR
4. 8259 Programmable Interrupt Controller
- Architecture and components
- Initialization vs operation command words
- Interrupt sequence with 8086
- Cascading multiple 8259s
