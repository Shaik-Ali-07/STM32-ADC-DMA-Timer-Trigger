# STM32-ADC-DMA-Timer-Trigger
A bare-metal STM32F411 project implementing a 100Hz ADC sampler using Timer triggers and DMA for zero-CPU data transfer to a Serial Terminal.
##  Project Objective
The goal is to implement a "Hardware-Only" trigger loop where:
1. A **Timer** acts as the heartbeat (100Hz).
2. The **ADC** samples automatically on every timer tick.
3. The **DMA** moves the result to memory instantly.
4. The **CPU** only wakes up to transmit the final data via **USART**.

### Hardware Configuration
- **MCU:** STM32F411CEU6 (Blackpill)
- **Core Clock:** 100MHz (via HSE + PLL)
- **Voltage Scaling:** Power Scale 1 enabled for high-frequency stability

### Peripheral Workflow
- **TIM2:** Configured as the Master Trigger. It generates a `TRGO` signal every 10ms (100Hz).
- **ADC1:** Set to "External Trigger" mode, listening to the Timer 2 TRGO event. It samples Channel 0 (PA0).
- **DMA2 Stream 0:** Operates in **Circular Mode**. It automatically moves data from `ADC1->DR` to a RAM variable as soon as a conversion completes.
- **USART6:** Configured at **115200 Baud** to stream results to a computer.(PA11 as Tx)
