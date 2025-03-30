# Nucleo F446RE
Test and demonstration program for Nucleo-F446RE development board.

**Note** The clock configuration in this project is different to the default config. (detail below)

### Timer based interrupt
TIM2 is configured to produce a 200kHz signal on PA10 (D2) pin.
TIM2 is driven by clock APB1 (clock config) at 90HMz it  counts up every 11.11111ns.

Prescaler PSC is 0 (no pre-scale)

The timing is configured via the ARR register. 
ARR = 225 x 11.111111 = 2500ns (time between interrupts)

The interrupt service routine toggles PA10, resulting a full period of 5us (200kHz) which can be observed with an oscilloscope PA10.

Timing accuracy depands on the clock source, the built 8MHz resonator is not very accurate. Fitting an 8MHz xtal X3 will improve accuracy significantly.

### Clock Source
Options for frequency reference (see Nucleo User Manual UM1724 data sheet 6.7.1 - OSC clock supply):

1. Onboard PLL frequency generator. This is the default configuration out of the box.
It is inaccurate and subject to temperature variations.

2. The MC0 from the STLINK module can be selected via board jumpers and is very accurate.  
To use this reference the following configuration is needed:
Note: this option can't be used if the STLINK board is removed.
Note: The MCO signal works fine with STLINK/V2-1 up to firmware V2J35M26.  
Any later firmware versions cause a delay of 2s before the MCO signal starts oscillation, this will prevent the CPU from starting correctly.

3. The PCB is not fitted with X3 by default, however, this can be retrofitted with an 8MHz crystal


The above options require the following modifications:

| Item | Opt 1 | Opt 2 | Opt 3 |
| --- | --- | --- | --- |
| SB16 | OFF | ON  | OFF |
| SB50 | OFF | ON  | OFF |
| SB54 | ON  | ON  | OFF |
| SB55 | OFF | OFF | OFF |
| R35  | removed | removed | 0R |
| R37  | removed | removed | 0R |
| C33  | removed | removed | 20pF |
| C35  | removed | removed | 20pF |

R35, R37, C33 and C35 are size 0603

 