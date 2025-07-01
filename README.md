# STM32 Interface Project: CAN, SPI, and I2C Communication

This project demonstrates the setup and implementation of **CAN**, **SPI**, and **I2C** communication protocols using STM32 microcontrollers. It consists of three labs, each focusing on a different communication protocol to interface multiple STM32 devices and external components. The goal is to establish reliable communication and demonstrate practical applications, such as data transfer, button-based messaging, and controlling LED blink rates.

---

## Lab 1: Setup CAN Bus Communication on STM32

### Objective
Establish **CAN Bus** communication between two STM32 microcontrollers and display the transmitted data on a laptop screen via UART. The setup uses a Saleae logic analyzer to capture and verify CAN frames.

### Hardware Components
- **STM32F103C8T6** microcontrollers (2 pcs)
- **MCP2551 CAN Transceiver** (2 pcs)
- **Saleae Logic Analyzer**
- **Jumper wires**
- **Breadboard**
- **USB-to-Serial adapter** (for UART communication)
- **Power supply** (5V for MCP2551, as STM32F103C8T6 via STLink does not provide 5V)

### Wiring
- **CAN Bus Connections**:
  - Connect **CAN_H** and **CAN_L** of the MCP2551 transceivers between the two STM32 boards.
  - Connect **CAN_Tx (PA9)** and **CAN_Rx (PA8)** of each STM32 to the respective pins on the MCP2551.
  - Ensure proper grounding (GND) between all components.
- **UART Connection**:
  - Connect STM32 UART pins (e.g., **PA9 (Tx)**, **PA10 (Rx)**) to a USB-to-Serial adapter for laptop communication.
 
- ![CAN Bus Diagram](https://github.com/hminh1012/STM32-CAN-SPI-I2C/blob/c841d2fb18201d45f5bef9dc7be5d483c1006108/workspace_CAN/CAN_diagram.png)
- **Logic Analyzer**:
  - Probe **CAN_Tx (PA9)** with the logic analyzer's **Data 0 (D0)** input.
  - Connect the logic analyzer's **GND** to the STM32 board's **GND**.


### Software Setup
- **CAN Configuration**:
  - Initialize the STM32 CAN peripheral in **Loopback Mode** first to verify the MCP2551 transceiver functionality (see [STM32F103 Loopback Mode Tutorial](https://community.st.com/t5/stm32-mcus/guide-to-can-bxcan-can2-0-configuration-in-loop-back-mode-on/ta-p/771119)).
  - Once verified, switch to **Normal Mode** for actual CAN communication.
  - Set the CAN bitrate to **500 kbps** (500000 bits/s).
- **UART Configuration**:
  - Configure UART (e.g., USART1 on PA9/PA10) to transmit received CAN data to the laptop.
  - Use a terminal program like **Hercules** to display the data.
- **Logic Analyzer Setup**:
  - Configure the Saleae logic analyzer to capture CAN frames at 500 kbps.
  - Monitor **CAN_Tx (PA9)** to verify frame transmission.

### CAN Frame Details
- ![CAN Frame Capture](https://github.com/hminh1012/STM32-CAN-SPI-I2C/blob/c841d2fb18201d45f5bef9dc7be5d483c1006108/workspace_CAN/CAN_frame.png)
- **Frame ID**: 0x123
- **Data Payload**: A sequence of 8 bytes representing the string "TXgui_@\n":
  | Value | Meaning | Description          |
  |-------|---------|----------------------|
  | 0x54  | T       | Start of sequence    |
  | 0x58  | X       | Second character     |
  | 0x67  | g       | Third character      |
  | 0x75  | u       | Fourth character     |
  | 0x69  | i       | Fifth character      |
  | 0x5F  | _       | Separator            |
  | 0x40  | @       | Special symbol       |
  | 0x0A  | \n      | Newline              |

- **Bit Stuffing**: The logic analyzer captures pulses related to zeroed data, indicating bit stuffing in the CAN protocol.
- **Verification**: The Hercules console displays the received data (`RxData`) as it changes.
- In the Hercules console we can see that RxData is changing ![Hercules Console Output](https://github.com/hminh1012/STM32-CAN-SPI-I2C/blob/dc674b567d946bc10370f56a0e8e6017c498c7cd/workspace_CAN/Hercules.png)

### Circuit

- ![CAN Setup Image](https://github.com/hminh1012/STM32-CAN-SPI-I2C/blob/dc674b567d946bc10370f56a0e8e6017c498c7cd/workspace_CAN/CAN_img.png)


> **_NOTE:_** The MCP2551 operates at 5V. If using STLink, provide an external 5V supply please follow as previous image in [report](https://github.com/hminh1012/STM32-CAN-SPI-I2C/blob/dc674b567d946bc10370f56a0e8e6017c498c7cd/workspace_CAN/lab1_CAN_BUSs.pdf), or power the STM32F103C8T6 via its USB port and connect 5V in STM32 with VCC in MCP2551.
> As shown as figure
>
> ![CAN Bus USB Power](https://github.com/hminh1012/STM32-CAN-SPI-I2C/blob/dc674b567d946bc10370f56a0e8e6017c498c7cd/workspace_CAN/USB_bus.png)

---

## Lab 2: Setup SPI Communication on STM32

### Objective
Implement **SPI** communication between one master STM32 (A) and two slave STM32s (B and C). The master uses two buttons to send messages ("A hello B" or "A hello C") to the respective slave. Each slave responds with "B hello A" or "C hello A" to the master.

### Hardware Components
- **STM32F103C8T6** microcontrollers (3 pcs: 1 master, 2 slaves)
- **Push buttons** (2 pcs for the master)
- **Jumper wires**
- **Breadboard**

### Wiring
- **SPI Connections**:
  - **Master (A)**: Configure SPI pins (e.g., **PA5 (SCK)**, **PA6 (MISO)**, **PA7 (MOSI)**).
  - **Slaves (B and C)**: Connect to the master's SPI lines.
  - Use separate **Chip Select (CS)** lines for each slave (e.g., **PA4 for Slave B**, **PA3 for Slave C**).
- **Button Connections**:
  - Connect two push buttons to the master’s GPIO pins (e.g., **PA0** and **PA1**).
  - One button triggers "A hello B", and the other triggers "A hello C".
- **Grounding**: Ensure all devices share a common **GND**.
- ![SPI Connection Diagram](https://github.com/hminh1012/STM32-CAN-SPI-I2C/blob/c841d2fb18201d45f5bef9dc7be5d483c1006108/workspace_SPI/SPI_diagram.png)

### Software Setup
- **Master Configuration**:
  - Initialize SPI in **Master Mode**.
  - Configure GPIO pins for buttons to detect presses.
  - Send "A hello B" or "A hello C" based on the button pressed, using the appropriate CS line to select the slave.
- **Slave Configuration**:
  - Initialize SPI in **Slave Mode**.
  - Upon receiving a message, respond with "B hello A" or "C hello A".
- **Data Transmission**:
  - Use SPI to transmit the message strings as byte arrays.
  - Ensure proper synchronization using the CS lines.

### Circuit

- ![SPI Setup Image](https://github.com/hminh1012/STM32-CAN-SPI-I2C/blob/c841d2fb18201d45f5bef9dc7be5d483c1006108/workspace_SPI/SPI_img.png)

> **_NOTE:_**
> - Proper grounding is critical to avoid communication errors.
> - Ensure the CS lines are correctly managed to prevent cross-talk between slaves.

---

## Lab 3: Setup I2C Communication on STM32

### Objective
Establish **I2C** communication between two STM32 microcontrollers, where each controls an LED’s blink rate based on the other’s potentiometer input. The master’s potentiometer controls the slave’s LED, and the slave’s potentiometer controls the master’s LED.

### Hardware Components
- **STM32F103C8T6** microcontrollers (2 pcs)
- **Potentiometers (10K Ohm)** (2 pcs)
- **LEDs** (2 pcs, with appropriate resistors)
- **Jumper wires**
- **Breadboard**

### Wiring
- **I2C Connections**:
  - Connect **SCL (PB6)** and **SDA (PB7)** between the master and slave STM32s.
  - Add pull-up resistors (e.g., 4.7kΩ) to **SCL** and ** SDA** lines, connected to 3.3V.
- **Potentiometer Connections**:
  - Connect one potentiometer to the master’s ADC pin (e.g., **PB1**).
  - Connect the other potentiometer to the slave’s ADC pin (e.g., **PB1**).
- **LED Connections**:
  - Connect an LED to a GPIO pin on the master (e.g., **PC13**).
  - Connect an LED to a GPIO pin on the slave (e.g., **PC13**).
- **Grounding**: Ensure a common **GND** between all components.
- ![I2C Connection Diagram](https://github.com/hminh1012/STM32-CAN-SPI-I2C/blob/c841d2fb18201d45f5bef9dc7be5d483c1006108/workspace_I2C/I2C_diagram.png)

### Software Setup
- **Master Configuration**:
  - Initialize I2C in **Master Mode**.
  - Read the potentiometer value using the ADC.
  - Transmit the value to the slave over I2C to control the slave’s LED blink rate.
  - Receive the slave’s potentiometer value to control the master’s LED blink rate.
- **Slave Configuration**:
  - Initialize I2C in **Slave Mode** (assign a unique I2C address, e.g., 0x12).
  - Read the potentiometer value using the ADC.
  - Transmit the value to the master and receive the master’s value to control the LED blink rate.
- **LED Control**:
  - Map the potentiometer ADC values (0–4095 for 12-bit ADC) to a suitable blink rate range (e.g., 100ms to 1000ms).

### Reference Code
- Use the I2C slave implementation from [ControllersTech: STM32 as I2C Slave](https://controllerstech.com/stm32-as-i2c-slave-part-1/).
- Example files: `i2c_slave.c`, `i2c_slave.h`.

### CAN Frame Details
- ![I2C Frame Capture](https://github.com/hminh1012/STM32-CAN-SPI-I2C/blob/c841d2fb18201d45f5bef9dc7be5d483c1006108/workspace_I2C/I2C_frame.png)
- ![I2C Logic Analyzer Log](https://github.com/hminh1012/STM32-CAN-SPI-I2C/blob/c841d2fb18201d45f5bef9dc7be5d483c1006108/workspace_I2C/I2C_log.png)

### Circuit

- ![I2C Setup Image](https://github.com/hminh1012/STM32-CAN-SPI-I2C/blob/c841d2fb18201d45f5bef9dc7be5d483c1006108/workspace_I2C/I2C_img.png)


### Notes
- The I2C setup is functional but not fully optimized. Future improvements could include:
  - Debouncing potentiometer readings for smoother control.
  - Adding error handling for I2C communication failures.
  - Optimizing the ADC sampling rate for better performance.
- Ensure pull-up resistors are properly sized to avoid signal integrity issues.
- Reference: [Circuit Basics I2C Tutorial](https://www.circuitbasics.com/how-to-set-up-i2c-communication-for-arduino/).

---

## General Notes
- **Power Supply**: Always verify the voltage requirements of components (e.g., MCP2551 requires 5V, STM32 operates at 3.3V).
- **Grounding**: A common ground is essential for all communication protocols to ensure reliable data transfer.
- **Debugging Tools**:
  - Use a logic analyzer (e.g., Saleae) to verify signal integrity and protocol timing.
  - Use a terminal program (e.g., Hercules) for UART output monitoring.
- **Development Environment**: Use STM32CubeIDE or a similar IDE for programming and debugging the STM32 microcontrollers.
- **Safety**: Double-check all connections to avoid short circuits or damage to components.

---

## Repository Structure
- **/workspace_CAN/**: Contains CAN-related diagrams, images, and resources.
- **/workspace_SPI/**: Contains SPI-related diagrams and images.
- **/workspace_I2C/**: Contains I2C-related diagrams, images, and logs.

## Credits
- [STM32F103 CAN Loopback Mode Tutorial](https://community.st.com/t5/stm32-mcus/guide-to-can-bxcan-can2-0-configuration-in-loop-back-mode-on/ta-p/771119)
- [ControllersTech STM32 I2C Slave](https://controllerstech.com/stm32-as-i2c-slave-part-1/)
- [Circuit Basics I2C Tutorial](https://www.circuitbasics.com/how-to-set-up-i2c-communication-for-arduino/)
