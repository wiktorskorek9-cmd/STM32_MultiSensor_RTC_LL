# STM32_MultiSensor_RTC_LL
# Low-Level Multi-Sensor Data Acquisition & RTC System (STM32-LL)

A bare-metal, low-level data acquisition system built for the **STM32F4** platform using the high-performance **STM32 LL (Low-Level) Drivers**. The project demonstrates efficient firmware architecture by implementing custom software-timed communication protocols (bit-banging), precise analog signal conversions, and a dual-interface UART command gateway.

## Hardware Peripherals Used

| Peripheral | Configuration Mode | Purpose |
| :--- | :--- | :--- |
| **ADC1** | 12-Bit, Right-Aligned, 480 Cycles | Samples analog voltage from the LM35 sensor |
| **TIM1** | Internal Clock Interrupt | General system timing base |
| **TIM2** | Variable Auto-Reload Base | Delays microsecond intervals for 1-Wire and 3-Wire bit-banging |
| **USART1** | 9600, 8N1, Polling / Async | Remote interface (e.g., Bluetooth transceiver) |
| **USART2** | 115200, 8N1, Rx Interrupt Mode | PC Terminal interface for live echoing and control |
| **GPIOA (Pin 0)** | Dynamic In/Out | Bidirectional single-wire signaling for DHT11 |
| **GPIOA (Pin 5)** | Digital Output | Status LED toggle pin controlled via serial interface |
| **GPIOB (Pins 1, 2, 15)** | Digital In/Out | Custom serial bus layout (Data, Clock, CE) for DS1302 RTC |

## Command Interface Protocol (CLI)

When controlling the system via the `USART1` transceiver interface, the system reacts to incoming characters on-the-fly:

* `'1'` - **Sensor Matrix Readout:** Triggers simultaneous execution of the `DHT11()` state-machine and `LM35()` ADC acquisition, streaming the parsed data back to the terminal interface.
* `'2'` - **RTC Timestamp Request:** Queries the `DS1302` time counters, decodes raw BCD buffers, and prints a perfectly formatted timestamp string (`HH:MM:SS`).
