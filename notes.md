# Macros

# Reset

## Reset pins
### Hard-reset
- PROGRAMN: initiates configuration sequence when asserted low. So reprograms the FPGA.
	- This is NOT a reset pin, this is simply a complete FPGA-ram delete
	- Logic type

```
Section 3.5:
Power-On-Reset (POR) puts the ECP5/ECP5-5G device in a reset state. POR is released when VCC, VCCAUX, and VCCIO8 are
ramped above the VPORUP voltage, as specified above.
VCCIO8 controls the voltage on the configuration I/O pins. If the ECP5/ECP5-5G device is using Master SPI mode to
download configuration data from external SPI Flash, it is required to ramp VCCIO8 above VIH of the external SPI Flash,
before at least one of the other two supplies (VCC and/or VCCAUX) is ramped to VPORUP voltage level. If the system cannot
meet this power up sequence requirement, and requires the VCCIO8 to be ramped last, then the system must keep either
PROGRAMN or INITN pin LOW during power up, until VCCIO8 reaches VIH of the external SPI Flash. This ensures the
signals driven out on the configuration pins to the external SPI Flash meet the VIH voltage requirement of the SPI Flash.
For LFE5UM/LFE5UM5G devices, it is required to power up VCCA, before VCCAUXA is powered up.
```

So the hard-reset voltage is controlled by the supply voltage for bank 8.

### Soft-reset
- RESET_N: simply supposed to set the FPGA to its initial state.
	- Butterstick doesn't have one
	- Logic type: 

## Reset VHDL
For some reason there are 3 resets, with each reset containing 3 registers. This to avoid metastability for the reset.

- Each reset register has a clock.
- 3 synchronization flip-flops pass the signal along the chain.
	1. Danger zone: first-flipflop
	2. Filter: samples first one, waits a clock cycle
	3. Not really necessary but extra safety

# Timing Constraints

