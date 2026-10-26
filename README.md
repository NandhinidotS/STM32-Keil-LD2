# STM32 NUCLEO-F446RE LD2 LED Blink using Keil MDK

## Project Overview

This project blinks the on-board green user LED (**LD2**) of the **STM32 NUCLEO-F446RE** board using **Keil MDK (µVision)** with **Arm Compiler 6**.

The microcontroller also sends status messages over UART to the PC, and these are read in a **PowerShell** window through the ST-LINK Virtual COM Port.

This was my first project in Keil MDK, and it confirmed that the build, flash and serial communication setup works.

## Key Features

- STM32 NUCLEO-F446RE board
- On-board LED (LD2) control
- Keil MDK (µVision) with Arm Compiler 6
- Toggle-based LED blinking
- UART serial output to the PC
- Serial data read in PowerShell, no extra terminal software needed
- Programming and debugging through the on-board ST-LINK

## Hardware Required

| Component | Quantity |
| --- | --- |
| STM32 NUCLEO-F446RE board | 1 |
| USB cable (data cable) | 1 |

No external components are needed. The LED is on the board.

## System Architecture

```
STM32F446RE Microcontroller
        ↓                ↓
  GPIO Pin PA5      USART2 (TX/RX)
        ↓                ↓
On-board LED (LD2)   ST-LINK Virtual COM Port
                         ↓
                  PC (PowerShell, COM26)
```

## Pin Configuration

| Signal | MCU Pin | Mode |
| --- | --- | --- |
| LD2 (green user LED) | PA5 | GPIO_Output |
| UART TX to PC | PA2 | USART2_TX |
| UART RX from PC | PA3 | USART2_RX |

## Keil Project Setup

1. Open **Keil µVision** and create a new project in `C:\keil_ws\LD2`.
2. Select the device **STM32F446RETx**.
3. Add the required software components (CMSIS Core, Device Startup, and the HAL GPIO/UART drivers if used).
4. Select **Arm Compiler 6** in `Options for Target → Target`.
5. In `Options for Target → Debug`, select **ST-Link Debugger**.
6. In `Options for Target → Debug → Settings → Flash Download`, enable **Reset and Run**.
7. Add `main.c` and build the project.

## Code

The program is written in `main.c`. The Keil window with the source code is shown below.

### Keil main.c

![Keil main.c](Keil_MainC.jpeg)

## Working

The program initializes the GPIO pin for LD2 and the UART peripheral. In the main loop it toggles the LED, sends a status message to the PC, and waits before repeating.

```
LED ON  → send message → wait → LED OFF → send message → wait → repeat
```

## Serial Output

The ST-LINK on the Nucleo board provides a Virtual COM Port, so no extra USB-to-serial adapter is needed. Check the COM port number in **Device Manager** (this project used **COM26**).

Run this command in PowerShell:

```powershell
$p=new-object System.IO.Ports.SerialPort COM26,115200,None,8,one; $p.Open(); while($true){$p.ReadLine()}
```

| Setting | Value |
| --- | --- |
| Port | COM26 (change to match your PC) |
| Baud rate | 115200 |
| Data bits | 8 |
| Parity | None |
| Stop bits | 1 |

Press `Ctrl + C` to stop reading. Run `$p.Close()` before opening the port again.

## Output

The green LED (LD2) on the board blinks continuously, and the PowerShell window prints the messages sent by the microcontroller.

### PowerShell Serial Output

![PowerShell Serial Output](PowerShell.jpeg)

## How to Run

1. Open the `.uvprojx` file in **Keil µVision**.
2. Connect the board to the PC with a USB cable.
3. Build the project with **F7**.
4. Download the program to the board with **F8**.
5. Press the black **Reset** button if the program does not start.
6. Open PowerShell and run the serial command above.

## Testing

| Test | Result |
| --- | --- |
| Build in Keil (Arm Compiler 6) | Successful |
| Flash through ST-LINK | Successful |
| LD2 blinking | Working |
| UART output in PowerShell | Working |

## Important Notes

### Jumpers

Both **CN2** jumper caps on the board must be fitted. If they are missing, the programmer cannot connect to the chip.

### COM Port

The COM port number can change between PCs or USB ports. Always check Device Manager before running the PowerShell command.

### One Program at a Time

Only one program can use the COM port at once. Close any other serial terminal before running the PowerShell command.

### USB Cable

Use a data-capable USB cable. Some cables only charge.

## Technologies and Concepts

- STM32 NUCLEO-F446RE
- Keil MDK (µVision)
- Arm Compiler 6
- GPIO output
- UART communication
- ST-LINK Virtual COM Port
- PowerShell serial reading
- ST-LINK programming and debugging

## Future Improvements

- DHT11 temperature and humidity monitoring
- LED current measurement using a current sensor
- Timer-based blinking without a software delay

# 🎥 Project Video

[▶️ Watch the LD2 Blink Demonstration](https://drive.google.com/file/d/1dUuqh_0AZzLrBXgQ_lnwNlmMUaVLctW_/view?usp=drivesdk)
