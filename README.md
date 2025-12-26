# LCD_test_Accelerometer

# AVR Multi-Channel ADC Monitor

This project demonstrates how to interface a 16x2 LCD with an AVR microcontroller (ATmega32/16) and read multiple analog inputs using the built-in ADC. It is designed to display three distinct sensor readings (labeled X, Y, and Z) simultaneously.

## Features
* **Multi-Channel Sampling:** Cycles through ADC channels 0, 1, and 2.
* **8-bit Precision:** Configured in Left-Adjust mode (`ADLAR`) for quick 8-bit reading from the `ADCH` register.
* **Interrupt Driven:** Uses ADC conversion complete interrupts to trigger subsequent readings.
* **LCD Feedback:** Formatted output on a standard 16x2 Character LCD.

## Hardware Configuration



### Pin Mapping
| Component | Microcontroller Pin | Function |
| :--- | :--- | :--- |
| **LCD Data (D0-D7)** | PORTC (0-7) | 8-bit Data Bus |
| **LCD RS** | PORTB Pin 0 | Register Select |
| **LCD RW** | PORTB Pin 1 | Read/Write |
| **LCD EN** | PORTB Pin 2 | Enable (Strobe) |
| **Analog Input X** | PORTA Pin 0 | ADC Channel 0 |
| **Analog Input Y** | PORTA Pin 1 | ADC Channel 1 |
| **Analog Input Z** | PORTA Pin 2 | ADC Channel 2 |

## Software Architecture
1. **`LCD_INIT()`**: Sends hex commands to initialize the LCD in 8-bit mode, clears the screen, and prints the static "X=", "Y=", and "Z=" labels.
2. **`CMD()` / `DATA()`**: Helper functions to toggle the Enable pin and switch between Instruction and Data registers.
3. **Main Loop**: 
    * Switches the `ADMUX` channel.
    * Starts conversion via `ADCSRA`.
    * Converts the result to a string via `itoa`.
    * Updates the specific coordinate on the LCD.
4. **ISR**: The `ADC_vect` ensures the `ADSC` (Start Conversion) bit is set again upon completion for continuous monitoring.

## Requirements
* **Compiler:** avr-gcc
* **Toolchain:** Atmel Studio or Makefile-based AVR-libc
* **Frequency:** 16MHz (defined as `F_CPU`)

## How to Use
1. Connect your sensors to Pins A0, A1, and A2.
2. Connect your 16x2 LCD to Port C and Port B as defined.
3. Compile the code and flash the `.hex` file to your microcontroller.
4. The LCD will display the real-time 8-bit value (0-255) of each sensor.
