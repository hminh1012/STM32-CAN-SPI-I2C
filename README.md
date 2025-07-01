# Lab Devices network: STM32 Interface Project

## Overview
# Lab 1:  Setup CAN BUS Communication on the STM32

Use 2 STM32 to communicate each other by CAN BUS and display on laptop screen using UART

![img1](https://github.com/hminh1012/STM32-CAN-SPI-I2C/blob/c841d2fb18201d45f5bef9dc7be5d483c1006108/workspace_CAN/CAN_diagram.png)
![img2](https://github.com/hminh1012/STM32-CAN-SPI-I2C/blob/c841d2fb18201d45f5bef9dc7be5d483c1006108/workspace_CAN/CAN_frame.png)
![img3](https://github.com/hminh1012/STM32-CAN-SPI-I2C/blob/c841d2fb18201d45f5bef9dc7be5d483c1006108/workspace_CAN/CAN_img.png)

> **_NOTE:_** Module CAN Bus MCP2551 operates at 5V (not 3.3V), but the STM32F103C8T6 does not provide 5V when connected to STLink. Therefore, you can refer to this diagram. Alternatively, you can remove STLink and power the STM32F103C8T6 through the USB port.


To check whether the CAN transceiver ic module is working or not, please run in Loopback mode. 

Cre: [Tutorial for Loopback mode STM32F103](https://community.st.com/t5/stm32-mcus/guide-to-can-bxcan-can2-0-configuration-in-loop-back-mode-on/ta-p/771119#:~:text=This%20article%20describes%20step%20by%20step%20how%20to,based%20on%20the%20STM32F103%20MCU%20%2F%20NUCLEO-F103RB%20board.)

If Loopback mode is working, just change Loopback mode to Normal mode in configuration part.
# Lab 2:  Setup SPI Communication on the STM32

Bidirectional communication between 1 master (A) and 2 slaves (B and C) using the SPI protocol. The master will have 2 buttons, each button when pressed will send the message "A hello B" or "A hello C" to the corresponding slave. When the slave receives the message, it will also reply with "C hello A" or "C hello B" to the master.

![img4](https://github.com/hminh1012/STM32-CAN-SPI-I2C/blob/c841d2fb18201d45f5bef9dc7be5d483c1006108/workspace_SPI/SPI_diagram.png)
![img5](https://github.com/hminh1012/STM32-CAN-SPI-I2C/blob/c841d2fb18201d45f5bef9dc7be5d483c1006108/workspace_SPI/SPI_img.png)

> **_NOTE:_** Wiring GND is important

# Lab 3: Setup I2C Communication on the STM32

Hardware Components: STM32 - 2pcs, Potentiometer (10K Ohm) - 2 pcs, Jumper wires, Breadboard. 

Adjust the potentiometer on the master device to control the blink rate of the slave device LED.
Adjust the potentiometer on the slave device to control the blink rate of the master device LED.

![img6](https://github.com/hminh1012/STM32-CAN-SPI-I2C/blob/c841d2fb18201d45f5bef9dc7be5d483c1006108/workspace_I2C/I2C_diagram.png)
![img7](https://github.com/hminh1012/STM32-CAN-SPI-I2C/blob/c841d2fb18201d45f5bef9dc7be5d483c1006108/workspace_I2C/I2C_frame.png)
![img8](https://github.com/hminh1012/STM32-CAN-SPI-I2C/blob/c841d2fb18201d45f5bef9dc7be5d483c1006108/workspace_I2C/I2C_img.png)
![img9](https://github.com/hminh1012/STM32-CAN-SPI-I2C/blob/c841d2fb18201d45f5bef9dc7be5d483c1006108/workspace_I2C/I2C_log.png)

Cre: [link](https://www.circuitbasics.com/how-to-set-up-i2c-communication-for-arduino/)

We refer i2c_slave.c and i2c_slave from [this link](https://controllerstech.com/stm32-as-i2c-slave-part-1/)

P/s: Lab 3 is not yet complete and optimized  but still meets the proposed requirements.


