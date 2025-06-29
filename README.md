# Lab Devices network: STM32 Interface Project

## Overview
# Lab 1:  Setup CAN BUS Communication on the STM32

Use 2 STM32 to communicate each other by CAN BUS and display on laptop screen using UART

# Lab 2:  Setup SPI Communication on the STM32

Bidirectional communication between 1 master (A) and 2 slaves (B and C) using the SPI protocol. The master will have 2 buttons, each button when pressed will send the message "A hello B" or "A hello C" to the corresponding slave. When the slave receives the message, it will also reply with "C hello A" or "C hello B" to the master.

# Lab 3: Setup I2C Communication on the STM32

Hardware Components: STM32 - 2pcs, Potentiometer (10K Ohm) - 2 pcs, Jumper wires, Breadboard. 

Adjust the potentiometer on the master device to control the blink rate of the slave device LED.
Adjust the potentiometer on the slave device to control the blink rate of the master device LED.

