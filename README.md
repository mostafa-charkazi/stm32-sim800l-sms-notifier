# STM32 SIM800L SMS Notifier

Blocking SMS notification firmware for STM32F103C8T6 (Blue Pill) using a
SIM800L GSM modem. The system sends predefined SMS alerts to multiple
stored phone numbers using raw AT commands over UART.

------------------------------------------------------------------------

## Hardware

-   MCU: STM32F103C8T6 (Blue Pill)
-   GSM Modem: SIM800L
-   3x LEDs (Active-High)
    -   PC13 → Network / Searching
    -   PC14 → Sending
    -   PC15 → Success
-   USART1 → SIM800L (115200 8N1)
-   USART2 → Debug / Trigger Console (115200 8N1)

------------------------------------------------------------------------

## Features

-   Robust AT command parser
-   Network registration check (AT+CREG?)
-   Blocking SMS transmission
-   Inline ARM Assembly for atomic GPIO control
-   No third-party GSM libraries

------------------------------------------------------------------------

## How It Works

1.  Disable echo: AT+ATE0

2.  Set SMS text mode: AT+CMGF=1

3.  Check network registration: AT+CREG?

4.  Send SMS to each stored number: AT+CMGS="NUMBER" MESSAGE + Ctrl+Z

------------------------------------------------------------------------

## LED Behavior

| State     | PC13 | PC14 | PC15  |
|-----------|------|------|-------|
| Searching | ON   | OFF  | OFF   |
| Sending   | OFF  | ON   | OFF   |
| Success   | OFF  | OFF  | ON    |
| Error     |    | Blink 5x |     |        
       
------------------------------------------------------------------------

## Compile-Time Configuration

Phone numbers and message are defined inside main.c:

static const char \*PHONE_NUMBERS\[\] = { "0905", "0991"};

static const uint8_t MESSAGE_TEXT\[\] = "Alert: System Triggered!";

Modify and rebuild to change recipients or message.

------------------------------------------------------------------------

## Limitations

-   SMS send only
-   No receive support
-   No GPRS/data
-   Blocking execution model
-   Fixed baud rate (115200)

------------------------------------------------------------------------

## Build Requirements

-   STM32CubeIDE (recommended)
-   STM32 HAL drivers
-   STM32F1 device support

------------------------------------------------------------------------

## Power Notes

SIM800L requires: - 3.8--4.2V supply - Up to 2A burst current - Large
low-ESR capacitor recommended

Unstable power may cause SMS failures or modem resets.
