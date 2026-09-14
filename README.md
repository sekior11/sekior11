# Mortal

**嵌入式软件工程师（2027 届）** · 技术主线：**MCU 低功耗设计**

自动化 · 本科

---

## 技术栈

| 方向 | 内容 |
| --- | --- |
| **低功耗（主线）** | STOP + RTC Alarm 周期唤醒、时钟门控按需上电、DMA Circular 零 CPU 采集、IO 模拟输入 + JTAG 释放 |
| **MCU / RTOS** | STM32（HAL / Keil MDK-ARM）、ESP32（ESP-IDF 5.4）、CH32V307（RISC-V）、FreeRTOS 多任务 / IPC / 内存管理 |
| **通信协议** | Modbus RTU（RS485 + CRC16）、I²C / SPI / UART / CAN、BLE（NimBLE）、Wi-Fi、USB 2.0 |
| **语言 / 工具链** | C / C++ / Python；Git、STM32CubeMX、ESP-IDF、PySide6、J-Link、示波器、逻辑分析仪 |

---

## 项目

### STM32 低功耗浮筒传感器固件（技术主线项目）

> 硬件产品固件，源码私有，可在面试中投屏讲解。

- **STOP 模式 + RTC Alarm 周期唤醒**：以 LSI 驱动的 5 Hz（200 ms）闹钟替代 `HAL_Delay` 阻塞轮询，主循环绝大部分时间停留在 STOP 模式
- **休眠前后的正确性处理**：进入 STOP 前按固定顺序清 `PWR_FLAG_WU` → 轮询 `RTC->CRL` 的 `RTOFF` → 清 `RTC_CRL_ALRF` → 清 `EXTI->PR` 第 17 位 → 设定下一个闹钟；唤醒后重新执行 `SystemClock_Config()` 恢复 HSI/PLL，再重启 DMA
- **DMA 零 CPU 采集**：USART3 `921600` 波特率 + DMA1_Ch3 循环模式，用「半满 / 全满」双回调搬运 11 字节 IWT603 帧；每次重新武装 DMA 前先清 USART 溢出标志，避免 `uart_errors` 单调累加
- **按需上电状态机**：FA66 称重模块仅在触发时上电，配合 2 s 建立稳定 → 500 ms 读取周期 → 1.5 g 阈值 → 3 s 滞回去抖，并同步门控 USART2 外设时钟
- **RTC 计数器作虚拟时间基准**：`(RTC->CNTH << 16) | RTC->CNTL` × 200 ms 得到绝对毫秒，避免长周期计时被休眠打断

### ESP32-S3 智能健康监测手环
[ESP32-Smart-Health-Monitoring-Devices](https://github.com/sekior11/ESP32-Smart-Health-Monitoring-Devices)

- MAX30102 **100 Hz** 采样 + **FFT_N=512** 频域分析：红光主频推心率、红外 AC/DC 比值算 SpO₂
- **NimBLE** 自定义 GATT（READ + NOTIFY）按 3 s 节拍推送 HR / SpO₂，采集 / FFT / 推送任务与协议栈独立调度
- BSP 组件化 + **LVGL** 多页面 UI（CST816S / MAX30102 / MPU6500 / GC9A01 驱动封装）

### CH32V307 高速 USB 虚拟示波器
[固件](https://github.com/sekior11/High-Speed-USB-Virtual-Oscilloscope) · [上位机 PySide6](https://github.com/sekior11/Host-computer-oscilloscope)

- 双 ADC 快速交替采样（TIM2 1 MHz 硬件触发）→ DMA1_Ch1 循环双缓冲 → **USB 2.0 High-Speed** 批量端点上传
- 上位机 `pyusb` 收流 + `pyqtgraph` 实时波形 / FFT / 触发 / 自动测量

### 蓝桥杯嵌入式省赛真题代码
[第 12 届](https://github.com/sekior11/12Province) · [第 13 届](https://github.com/sekior11/13Province) · [第 15 届](https://github.com/sekior11/15Province)

- STM32 + CubeMX + HAL，三届题目均通过 4T 评测网实测

---

## 开源贡献

**[FreeRTOS-Kernel PR #1492](https://github.com/FreeRTOS/FreeRTOS-Kernel/pull/1492)** · 状态：**OPEN，等待上游 review**（未合并）

- 定位并修复 IAR TrustZone non-secure 端口 `portasm.s` 的汇编缺陷：误粘的安装包 URL 落在 `/* */` 注释之外，汇编器将其当作 `ldmia` 的操作数，导致 ARMv8-M 非安全端口构建直接失败
- 上游 issue #1480 只报告了 1 个文件，`main` 分支全树扫描实测**受影响文件共 7 个**（均在 476 行）；本 PR 覆盖其中 6 个，与另一贡献者的 #1485 构成 **1 + 6 无重叠**划分
- 用 `arm-none-eabi-gcc 13.3.1` 搭最小汇编验证：6/6 文件修复前报错、修复后通过（已知局限：非 IAR 官方工具链实测）
- 用 `git log -S` 将缺陷溯源到引入提交 78e0cc7，并区分出「直接引入」与「由已受影响文件继承」两类来源

---

## 联系

- 邮箱：13603944315@163.com
- 微信：mss13603944315
