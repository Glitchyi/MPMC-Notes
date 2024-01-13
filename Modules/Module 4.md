- 8255: It is a programmable peripheral input/output device
- 8256: Programmable interval timer 
- 8257: DMA controller 

## 8255
it is a programmable general purpose peripheral io device, this device is 


## 8253/8254
Programmable interval Timer

MP require some delay between some operations, they can be brought about by using software or hardware. 
- **Software delays** : Software delays can be achieved by making a delay subroutine which will start to decrement a count value , which when it reaches zero the subroutine can be cancelled.
- **Hardware Delay/Timer delay**: External timers can be used to maintain timing, the timer interrupts the process at intervals, this method is more efficient as CPU is not busy decrement the count value in the case of software register. Another advantage is the processor can execute other task in between the interrupts

### 8254 
it is a interval timer made by intel which facilitate the generation of accurate time delays, making use of this reduces software overhead and variable length delays can be made. 

>8254 contains three independent 16-bit counter each with a maximum count rate of 10 MHz

This allows for three counters to  be used simultaneously which can be controlled by programming internal command word registers.

### Application of 8254
- To generate timing delays
- To generate clock pulses 
- As a square wave generator

### Architecture
![[Images/Pasted image 20231130185349.png]]

**Data bus buffer** 
- This is a 3 state bi directional 8 bit buffer, which is used to interface the 8254 to the system bus. 
- Data transmitted by using the IN and OUT instructions.

**Read write logic**
- This controls the direction of the data buffer depending open if its a read or write operation.
- It takes input signals from the system bus and generates control signals accordingly for the other blocks.
- A1 and A0 help select the counter or the control word register to be used for i/o

| CS  | RD  | WR  | A1  | A0  | Operation          |
| --- | --- | --- | --- | --- | ------------------ |
| 0   | 1   | 0   | 0   | 0   | Write Counter 0    |
| 0   | 1   | 0   | 0   | 1   | Write Counter 1    |
| 0   | 1   | 0   | 1   | 0   | Write Counter 2    |
| 0   | 1   | 0   | 1   | 1   | Write Control Word |
| 0   | 0   | 1   | 0   | 0   | Read  Counter 0    |
| 0   | 0   | 1   | 0   | 1   | Read  Counter 1    |
| 0   | 0   | 1   | 1   | 0   | Read  Counter 2    |
| 0   | 1   | 0   | 1   | 1   | No Operation       |
| 0   | 1   | 1   | X   | X   | No Operation       |
| 1   | X   | X   | X   | X   | Disabled           |

**Control Word Register**
the control word is the block responsible for selecting one of the six operating modes. the microprocessor has to initialize the counter to decide its operating mode by writing the control word to the register. It cant  be read from.

**Counters**
The three counters in 8254 are independent 16 bit presettable down counters able to operate in BCD or hexadecimal mode.  The mode of operation is set to IN/OUT by the control register. The specialty of 8254 counter is that they can be easily read on line without disturbing the clock inputs to the counter. The facility is called as on the fly reding of counter using a mode control word. There are three pins for each counter:
- CLK input clock frequency
- OUT used to get the different  waves like  one shot, or square wave with different duty cycles
- Gate used to enable or disable the counter.

**General counter operation**

16 bit counter value will be loaded onto the the counter register which starts decrementing when the command is given, when the count is 0 then an interrupt will be raised, the counter value can be checked during decrementing too.

| Mode | Description                                    | Note                                                                                                                                                                                                        |
| ---- | ---------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0    | Interrupt on terminal count                    | - OUT pin is initially low <br>- The count value is loaded <br>- GATE pin is high <br>- During counting OUT remains low<br>- After count over, OUT becomes high and remains high<br>- GATE low means pause, high means resume |
| 1    | Hardware Retriggerable One-Shot |- OUT pin is initially high <br>- The count value is loaded <br>- Counting only begins when rising edge applied to GATE <br>- During counting OUT goes low remains low<br>- After count over, OUT becomes high and remains high<br>- GATE low means no effect, low then high means retrigger |
| 2    | Rate Generator                                 | - OUT pin is initially high <br>- The count value is loaded <br>- GATE pin made high <br>- During counting OUT goes low for 1 cycle just before count ends<br>- GATE low means disable, low then high means restart<br>- Also known as a divide by n counter, output frequency = input frequency/n|
| 3    | Square Wave Generator                          |- OUT pin is initially high <br>- The count value is loaded <br>- GATE pin made high <br>- During counting OUT remains high for half of the cycle and low for the remaining half<br>- On completion the count is reloaded and the makes continuous loop <br>- GATE low means disable, low then high means restart<br>- Also known as a divide by n counter, output frequency = input frequency/n|
| 4    | Software Triggered Strobe                      |  - OUT pin is initially high <br>- The count value is loaded <br>- GATE pin made high <br>- During counting OUT remains high<br>- The OUT pin goes low for one clock cycle after completion<br>- If GATE made low, disables and restarts counting when made high again|
| 5    | Hardware Triggered Strobe                      |- OUT pin is initially high <br>- The count value is loaded <br>- GATE needs a trigger to start counting, need not remain high <br>- During counting OUT remains high<br>- The OUT pin goes low for one clock cycle after completion<br>- GATE used as used as trigger to start counting, and retrigger|



## 8257
Direct memory acces is a way to directly acces the memory by bypassing the CPU, this is managed by a chip called as the DMAC, The DMA acts as an intermediary between the devices and memory and accepts DMA requests (DMQ).
The DMA controller send a Hold request to the CPU and waits for the CPU to assert the HLDA (hold acknowledgement), the microprocessor tristate all the data bus, address bus and control (disconnects the lines from the microprocessor) and when the acknowledgment is received the DMA controller has to manage the operations over buses between the CPU memory and I/O devices.
![[Images/Pasted image 20231130214239.png]]
