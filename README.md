#Nucleo F446RE
Test and demonstration program.

**Note** The clock configuration in this project is different to the default config.

###Timer based interrupt
TIM2 is configured to produce a 200kHz signal on PA10 (D2) pin.
TIM2 is driven by clock APB1 (clock config) at 90HMz it  counts up every 11.11111ns.

Prescaler PSC is 0 (no pre-scale)

The timing is configured via the ARR register. 
ARR = 225 x 11.111111 = 2500ns (time between interrupts)

The interrupt service routine toggles PA10, resulting a full period of 5us (200kHz) which can be observed with an oscilloscope PA10.

Timing accuracy depands on the clock source, the built 8MHz resonator is not very accurate. Fitting an 8MHz xtal X3 will improve accuracy significantly.


 