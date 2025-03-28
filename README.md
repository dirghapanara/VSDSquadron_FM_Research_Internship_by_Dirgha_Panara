# VSDSquadron_FM_Research_Internship_by_Dirgha
The VSDSquadron FPGA Mini (FM) is a compact and low-cost development board designed for FPGA prototyping and embedded system projects. This board provides a seamless hardware development experience with an integrated programmer, versatile GPIO access, and onboard memory, making it ideal for students, hobbyists, and developers exploring FPGA-based designs.
![FPGA Mini Board](https://github.com/user-attachments/assets/e6b3f3ea-f00d-42de-997c-d0a3c5b4579f)

## Features of the FM board:
- **FPGA:**
   * Powered by the Lattice UltraPlus ICE40UP5K FPGA
   * Offers 5.3K LUTs, 1Mb SPRAM, 120Kb DPRAM, and 8 multipliers for versatile design
     capabilities
- **Connectivity:**
   + Equipped with an FTDI FT232H USB to SPI device for seamless communication
   + All FTDI pins are accessible through test points for easy debugging and customization
- **General Purpose I/O (GPIO):**
   * All 32 FPGA GPIOs brought out for easy prototyping and interfacing
- **Memory:**
   + Integrated 4MB SPI flash for data storage and configuration
- **LED Indicators:**
   + RGB LED included for status indication or user-defined functionality
- **Form Factor:**
   + Compact design with all pins accessible, perfect for fast prototyping and embedded applications

## Features of the FM board:
The block diagram shown in Figure depicts the key components of the VSDSquadron FPGA Mini(FM) board.

![block_diagram](https://github.com/user-attachments/assets/0f452595-18bc-4ce3-b19b-20be45e8d2ba)

# Task 1: Understanding and Implementing the Verilog Code on FM
## Objective: 
Understanding and documenting the provided Verilog code, creating the necessary PCF file, and integrating the design with the [VSDSquadron FPGA Mini Board](https://www.vlsisystemdesign.com/vsdsquadronfm/) using the provided [datasheet](https://www.vlsisystemdesign.com/wp-content/uploads/2025/01/VSDSquadronFMDatasheet.pdf)

## Contents:
### Step 1: Understanding the Verilog code
The Verilog code can be accessed from the github repo at the [LINK](https://github.com/thesourcerer8/VSDSquadron_FM/tree/main). This repository contains projects for VSDSquadron_FM, utilizing open-source FPGA tools for development. It demonstrates implementing the hardware module that controls RGB LEDs using an internal oscillator and counter.

### Port Analysis:
![image](https://github.com/user-attachments/assets/91b214a4-9f0b-40c9-9ffe-1f2bdb33d3e2)

The first section of the code specifies the **ports** of the board, which are in the form of:
1. *led_red, led_blue, led_green* : These **three output wires** control the **RGB LED colors**. Each wire carries a **single-bit signal** that determines whether its corresponding color is **active** (1) or **inactive** (0).
2. *hw_clk* : A **single-bit input wire** that connects to the **hardware oscillator**, providing the system **clock signal** that drives the module's timing.
3. *testwire* : A **single-bit output** that provides a **test/debug signal**, specifically connected to **bit 5** of the frequency counter.

### Internal Component Analysis:
The module specifies three main internal components:

**1. Internal Oscilliator (*SB_HFOSC*)**
- Purpose: This generates a stable internal clock signal
- Configuration: Uses CLKHF_DIV = "0b10" (binary 2) for clock division
- Control Signals:
    1. *CLKHFPU = 1'b1* : Enables power-up
    2. *CLKHFEN = 1'b1* : Enables oscillator
    3. *CLKHF* : Output connected to internal *int_osc* signal

**2. Frequency Counter Logic**
- Implementation: 28-bit register (*frequency_counter_i*)
- Operation: Increments on every positive edge of *int_osc*
- Test functionality: Bit 5 is routed to *testwire* for monitoring
- Purpose: Provides a way to verify oscillator operation and timing

**3. RGB LED Driver (*SB_RGBA_DRV*)**

- Configuration:
    1. *RGBLEDEN = 1'b1* : Enables LED operation
    2. *RGB0PWM = 1'b0* : Red LED minimum brightness
    3. *RGB1PWM = 1'b0* : Green LED minimum brightness
    4. *RGB2PWM = 1'b1* : Blue LED maximum brightness
    5. *CURREN = 1'b1* : Enables current control
- Current settings: All LEDs set to "0b000001" (minimum current)
 - Output connections:
    1. *RGB0* → *led_red*
    2. *RGB1* → *led_green*
    3. *RGB2* → *led_blue*
### Module Documentation (Summary)
**Purpose**

This Verilog module implements an RGB LED controller with internal timing capabilities. It provides a stable internal clock source and manages RGB LED outputs while maintaining testability through a dedicated test signal. It provides a complete solution for RGB LED control with built-in timing and test capabilities, and is suitable for embedded systems requiring stable LED operation with minimal external dependencies.

**Internal Logic and Oscillator**

The module uses a high-frequency oscillator (*SB_HFOSC*) as its internal timing source. The oscillator's output drives a 28-bit frequency counter, which provides both timing information and a test signal. The counter's bit 5 is routed to the *testwire* output for external monitoring.

**RGB LED Driver Functionality**

The RGB LED driver (SB_RGBA_DRV) manages the LED outputs with the following characteristics:

- Current-controlled outputs with minimum current setting ("0b000001")
- Pulse Width Modulation (PWM) control for each color channel
- Fixed configuration:
    1. Blue LED at maximum brightness (*RGB2PWM = 1'b1*)
    2. Red and Green LEDs at minimum brightness (*RGB0PWM = RGB1PWM = 1'b0*)

### Step 2: Creating the PCF file
The PCF [Physical Constraint File] file can be accessed [here](https://github.com/thesourcerer8/VSDSquadron_FM/blob/main/led_red/VSDSquadronFM.pcf). It is used in FPGA development to map logical signals from HDL code to physical pins on the FPGA chip. Each *set_io* command establishes this connection between the named logical ports to physical pins on the board.

### Map of logical signal with physical pins:

### 1. *set_io led_red 39*
This command maps the logical signal *led_red* to the physical pin 39, allowing the HDL code to control a LED (color red is implied) connected to pin 39.

### 2. *set_io led_blue 40*
This command maps the logical signal *led_blue* to the physical pin 40, allowing the HDL code to control a LED (color blue is implied) connected to pin 40.

### 3. *set_io led_green 41*
This command maps the logical signal *led_green* to the physical pin 41, allowing the HDL code to control a LED (color green is implied) connected to pin 41.

### 4. *set_io hw_clk 20*
This command assigns the hardware clock signal *hw_clk* to the physical pin 20, allowing the HDL code to recieve clock inputs through pin 20.

### 5. *set_io testwire 17*
This command maps *testwire* to pin 17, for testing or debugging purposes.
### Step 3: Integrating with the VSDSquadron FPGA Mini Board
    
### Steps to Follow (based on instructions)
1. Reviewing the [VSDSquadron FPGA Mini board datasheet](https://www.vlsisystemdesign.com/wp-content/uploads/2025/01/VSDSquadronFMDatasheet.pdf) to understand its features and pinout.
2. Using the [datasheet](https://www.vlsisystemdesign.com/wp-content/uploads/2025/01/VSDSquadronFMDatasheet.pdf) to correlate the physical board connections with the PCF file and Verilog code.
3. Connecting the board to the computer as described in the datasheet (e.g., using USB-C and ensuring FTDI connection).
4. Following the [Makefile](https://github.com/thesourcerer8/VSDSquadron_FM/blob/main/blink_led/Makefile)
 for building and flashing the Verilog code:
   - Run 'make clean' to clear any previous builds
     Observe that only RED led is ON after clean is executed
   - Run 'make build' to compile the design
   - Run 'sudo make flash' to program the FPGA board
5. Observe the behavior of the RGB LED (blinks) on the board to confirm successful programming.

> [!IMPORTANT]
> If FPGA program flash fails, check the connectivity between board and your computer<br/>
> Check if the USB cable is not faulty

### Board after successful program flash

https://github.com/user-attachments/assets/795ab613-1f0a-4cd5-b3ee-9c760dcb57b0

### Step 4: Final Documentation:
    
### Summary of the Verilog code functionality
This Verilog module controls an RGB LED with an internal high-frequency oscillator (SB_HFOSC) and a 28-bit frequency counter. The counter's bit 5 is routed to a testwire for monitoring. The RGB LED driver (SB_RGBA_DRV) provides current-controlled PWM outputs with a fixed configuration: blue at maximum brightness, red and green at minimum. It ensures stable LED operation with minimal external dependencies, making it ideal for embedded systems.

### Pin mapping details from the PCF file
The file maps a red LED to pin 39, a blue LED to pin 40, a green LED to pin 41, a clock signal to pin 20, and lastly a testwire to pin 17. This also corresponds with the [datasheet](https://www.vlsisystemdesign.com/wp-content/uploads/2025/01/VSDSquadronFMDatasheet.pdf).

### Understanding and Implementing the Verilog Code 
1. Follow the datasheet and install all the tools needed (Yosys for logic synthesis, NextPNR for placement and routing, IceStorm for Bitstream generation, and Git for Version control)
2. Verify the physical board connections between the PCF file and Verilog code
3. Connect the board to the computer as described in the datasheet (e.g., using USB-C and ensuring FTDI connection).
4. Follow the [Makefile](https://github.com/thesourcerer8/VSDSquadron_FM/blob/main/blink_led/Makefile) for building and flashing the Verilog code: (run the following steps)
     - Run 'make clean' to clear any previous builds
     - Run 'make build' to compile the design
     - Run 'sudo make flash' to program the FPGA board
5. Verify that LED remains BLINKS (controlled by SB_RGBA_DRV) - this is the expected result:

https://github.com/user-attachments/assets/40b9e7ab-f480-4d6a-ad70-fb5d0563cedc

### Tips and Tricks:
- Make sure Virtual Box is installed properly, check datasheet for the detailed steps.
- Check the USB cable is working properly
- Use google for detail information on Verilog.
- Help is just one message away!!!
