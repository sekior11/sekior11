# Mortal · 马森森

**嵌入式软件工程师（2027 届）** · 技术主线：**MCU 低功耗设计**

河南工学院 · 自动化 · 本科

---

## 技术栈

| 方向 | 内容 |
| --- | --- |
| **低功耗（主线）** | STOP + RTC Alarm 周期唤醒、时钟门控按需上电、DMA Circular 零 CPU 采集、IO 模拟输入 + JTAG 释放 |
| **MCU / RTOS** | STM32（HAL / Keil MDK-ARM）、ESP32（ESP-IDF 5.4）、CH32V307（RISC-V）、FreeRTOS 多任务 / IPC / 内存管理 |
| **通信协议** | Modbus RTU（RS485 + CRC16）、I²C / SPI / UART / CAN、BLE（NimBLE）、Wi-Fi、USB 2.0 |
| **语言 / 工具链** | C / C++ / Python；Git、STM32CubeMX、ESP-IDF、PySide6、J-Link、示波器、逻辑分析仪 |

## 项目

### ESP32-S3 智能健康监测手环
[ESP32-Smart-Health-Monitoring-Devices](https://github.com/sekior11/ESP32-Smart-Health-Monitoring-Devices)

- MAX30102 **100 Hz** 采样 + **FFT_N=512** 频域分析：红光主频推心率、红外 AC/DC 比值算 SpO₂
- **NimBLE** 自定义 GATT（READ + NOTIFY）按 3 s 节拍推送 HR / SpO₂，采集 / FFT / 推送任务与协议栈独立调度
- BSP 组件化 + **LVGL** 多页面 UI（CST816S / MAX30102 / MPU6500 / GC9A01 驱动封装）

### CH32V307 高速 USB 虚拟示波器
[High-Speed-USB-Virtual-Oscilloscope](https://github.com/sekior11/High-Speed-USB-Virtual-Oscilloscope) · [上位机 PySide6](https://github.com/sekior11/Host-computer-oscilloscope)

- 双 ADC + DMA 采集，**USB 2.0 High-Speed** 高速传输
- 上位机基于 **PySide6**，实时波形显示与数据解析

### 蓝桥杯嵌入式省赛真题代码
[第 12 届](https://github.com/sekior11/12Province) · [第 13 届](https://github.com/sekior11/13Province) · [第 15 届](https://github.com/sekior11/15Province)

- STM32 + CubeMX + HAL，三届题目均通过 4T 评测网实测

## 开源贡献

- **[FreeRTOS-Kernel PR #1492](https://github.com/FreeRTOS/FreeRTOS-Kernel/pull/1492)** — 定位并修复 IAR TrustZone non-secure 端口 `portasm.s` 的汇编缺陷：误粘的安装包 URL 落在 `/* */` 注释之外，汇编器将其当作 `ldmia` 的操作数，导致 ARMv8-M 非安全端口构建失败。实测受影响文件为 **7 个**（上游 issue 仅报告 1 个），并用 `git log -S` 溯源至引入提交。

## 联系

- 邮箱：13603944315@163.com
- 微信：mss13603944315
