# FreeRTOS Multi-Task Sensor Logger — STM32F446RE

A preemptive multitasking data logger running on an STM32 Nucleo board. 
Reads live accelerometer data from an MPU6050 sensor over I2C and streams 
it to a PC over UART, with three concurrent FreeRTOS tasks running 
independently at different rates.

## Demo

Connect the board, open a serial monitor at 115200 baud, and tilt the 
sensor — X Y Z acceleration values update in real time.

## How it works

Three FreeRTOS tasks run concurrently:

| Task | Rate | Job |
|------|------|-----|
| Sensor Task | Every 10ms | Reads MPU6050 accelerometer over I2C |
| Logger Task | On data arrival | Formats and sends readings over UART to PC |
| LED Task | Every 500ms | Blinks onboard LED to show scheduler is alive |

A **queue** connects the Sensor and Logger tasks so data is passed safely 
between them without shared memory issues. A **mutex** protects the I2C 
bus so only one task can use it at a time.

## Hardware

- STM32 Nucleo-F446RE
- MPU6050 accelerometer/gyroscope module

## Wiring

| MPU6050 | Nucleo |
|---------|--------|
| VCC | 3.3V |
| GND | GND |
| SDA | PB9 |
| SCL | PB8 |
| AD0 | GND |

## Tools

- STM32CubeIDE + STM32CubeMX
- FreeRTOS (CMSIS V2)
- STM32 HAL
- PuTTY for serial monitoring
