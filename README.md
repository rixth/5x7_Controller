# 5x7 Controller

This is a project that provides hardware & firmware for a small (43x70mm) board that contains four 5x7 row-anode LED matrices.

## Controller primary components

* 4 5x7 row-anode LED matrices
* 2 Texas Instruments TLC59284 sixteen-channel LED driver (constant-current sink)
* 1 CD4022B 8-output counter
* 1 74HC540 inverting buffer
* 7 IRLML5203 P-channel mosfets
* Freescale Kinetis MKL02Z32VFM4 Cortex M0+ MCU (32k flash)
* So. Many. Interfaces.

## Brief design overview

This display is drawn row-by-row. The row to be drawn is selected by the counter, which feeds it output through an inverting-buffer, then out to the P-channel mosfets. The columns are selected by the TLC59284 which are essentially shift registers that can sink considerable (configurable) current, so you can avoid having many current limiting resistors for the LEDs.

The Kinetis MCU will be responsible for drawing the display, and receiving commands from the main controller. I have exposed the MCU's i2c, SPI and UART lines to headers.

The raw inputs for the row/column selection have also been broken out to the left side of the board - 4 pins are required (shift clk, in, latch + counter clk). This can be reduced by 1 if a solder bridge is used to tie the clock of the counter to the latch of the shift register.

## Software overview

The software is still in the planning phases, but the rough plan is to use a slim RTOS like [AtomThreads](http://atomthreads.com/) to both keep the display refeshed and take input from the host controller over i2c, SPI or serial.

The MCU will have an ASCII character set preloaded - but the command set will also allow the host to toggle single LEDs, rows and columns. 