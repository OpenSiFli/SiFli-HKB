---
title: DS5702-SF32LB57X 芯片技术规格书 V0.2.2（2026-09-24）
type: mcu_source
aliases: ["SF32LB57X_Datasheet_V0.2.2","DS5702-SF32LB57X-CN","SF32LB57X规格书V0.2.2"]
tags: ["企业MCU","来源文档","规格书/datasheet","芯片系列-SF32LB57","3.3V供电系列"]
doc_type: datasheet
doc_version: V0.2.2
doc_date: 2026-09-24
doc_status: 草稿（0.0.0~0.9.9，非正式发布，参数随时可能更改）
vendor: 思澈科技（南京）有限公司 SiFli
sources: ["[[raw/mcu/all_chip/datasheets/DS5702-SF32LB57X-芯片技术规格书-V0p2p2-20261102.pdf]]"]
mcu_family: ["SF32LB57"]
mcu_series: ["3.3V供电系列"]
mcu_model: ["SF32LB57X"]
hardware_version: ["QFN68 7x7","QFN80 8x8","BGA112 5.4x4.4","Datasheet V0.2.2"]
created: 2026-11-02
updated: 2026-11-02
---

## 摘要
SiFli **SF32LB57X（大写 X）= 3.3V 供电系列** 芯片技术规格书。文档编号 **DS5702-SF32LB57X-CN**，版本 **V0.2.2**（2026-09-24 更新），PDF 共 84 页。

> 📌 **系列判定**：`DS5702-SF32LB57X`（大写 X）= 3.3V 系列；电池系列为 `DS5701-SF32LB57x`（小写 x）。与 SF32LB52 家族规则相同。

## 文档状态与版本演进
- 本版为 **草稿（V0.2.2）**：0.0.0~0.9.9 为初稿，信息为初步数据，**不能保证准确性，随时可能更改，思澈科技不主动通知**。
- **更新历史（PDF p2）**：
  - 2026-05-29 0.1 初稿
  - 2026-08-29 0.2 更新封装信息，IO 信息，蓝牙相关指标
  - 2026-09-16 0.2.1 封装信息勘误
  - **2026-09-24 0.2.2 增加了 CPU 以及蓝牙的功耗数据**（即 Table 4-5 / Table 4-6 由 TBD 变为实测值）
  - 此外 **Table 4-1 / Table 4-2 已精简**：3.3V 册仅保留本系列实际供电 **PVDD（2.97~3.63V）**，不再列 VBUS/VBAT/VCC（V0.2 曾把 die 双供电参数一并列出，V0.2.2 按封装实际供电清理；布线仍以引脚表为准）。

## 芯片核心规格（本册）
- 内核：双 Arm Cortex-M33 STAR-MC1 @240MHz（HPSYS 双性能核），1968 CoreMark
  - HCPU：32KB(2-way) I$ + 16KB(4-way) D$；ACPU：16KB(2-way) I$ + 8KB(4-way) D$
  - HPSYS SRAM 512KB（全部 Retention）；LPSYS Cortex-M33 @24MHz + SRAM 80KB（全部 Retention）；合计 **592KB**
- 图形：2D/2.5D **ePicasso 3.0** 引擎，最大 2048×2048；**eZip 3.0** 解压加速器
- 显示：LCD 控制器支持 8080 / SPI·Dual·Quad-SPI / DPI(RGB) / JDI / 8-16b EPD；DCMI 摄像头
- 无线：双模蓝牙 **6.3**（BLE Audio、LE LR）；BLE 灵敏度 ‑100dBm(1Mbps) / ‑107.5dBm(S8)；最大发射 17dBm(BR/BLE) / 13dBm(EDR2/3)
- 音频：1×24-bit DAC（SNR 109dB）、2×24-bit ADC（SNR 97dB）、Micbias LDO 1.4~2.8V/0~2mA
- 存储：SiP NOR-Flash ≤104MHz、SiP OPI-PSRAM ≤156MHz；3×MPI(QSPI)、2×SD/SDIO（SD3.0/SDIO3.0/eMMC）
- 外设：58 GPIO + 6 GPO MPI IO；3×UART、4×I2C、2×SPI、2×I2S、2×PDM、1×USB2.0 FS、**2×CAN**、GPDMA、PTM
- 模拟：1×12-bit SAR ADC（12 通道，4Msps）、1×片上温度传感器
- 定时器：2×16b GPTIM、4×32b BTIM、2×32b ATIM、2×24b LPTIM、1×PWM、1×RTC、2×24b WDT、1×IWDT
- 安全：AES/HASH/CRC、TRNG、eShield、PSA Certified Level 1
- 电源管理：内置高效率 Buck + 低功耗 LDO；**提供两路对外 3.3V 供电，每路最大 150mA**；休眠功耗 Deepsleep 28µA（V0.2.2 实测，见功耗表）
- 工作温度：‑40 ~ +85 °C
- 封装与 GPIO：QFN68 7×7（47 GPIO）／QFN80 8×8（58 GPIO）／BGA112 5.4×4.4（≤58 GPIO + 6 GPO MPI IO）

## 关键章节页码索引（V0.2.2 PDF 页）
| 内容 | PDF 页 | 备注 |
|---|---|---|
| 更新历史 / 文档状态 | 2 | 草稿声明 + 版本演进 |
| 产品概述 / 功能框图 | 3 | |
| 产品特性 | 4–5 | CPU/图形/无线/音频/外设/封装 |
| 目录 / 插图 / 表格 | 7–10 | |
| 常用接口速率（Table 3-1） | 38 | MPI 156/104MHz、SDMMC 48MHz、I2C 3.4MHz、SPI 48MHz、UART 3Mbaud、GPADC 4Msps |
| CAN 特性 | 38 | CAN2.0b，不支持 CAN FD，最高 1Mbps |
| 运行条件 Table 4-1 | 37 | **仅 PVDD 2.97~3.63V**（V0.2.2 精简，不再含 VBUS/VBAT/VCC） |
| 绝对最大值 Table 4-2 | 37 | **PVDD 3.63V**、Tstorage ‑40~125°C |
| I/O 特性 @3.3V Table 4-3 | 37 | IOH/IOL 12/30/38mA、RPU/RPD 7/10/20kΩ |
| 可靠性测试 Table 4-4 | 38 | HBM ±4000V、CDM ±1000V、MSL3 |
| 处理器功耗 Table 4-5 | 39 | ✅ **V0.2.2 新增实测（@3.8V）** |
| BT/BLE 功耗 Table 4-6 | 39 | ✅ **V0.2.2 新增实测（@3.3V）** |
| BLE 射频 TX/RX Table 4-7~4-12 | 40–43 | 1M/2M/S2/S8 |
| 经典蓝牙 TX/RX Table 4-13~4-17 | 44–47 | BR/EDR |
| 音频 ADC Table 4-18 | 47 | 24bit、SNR 97dB |
| 音频 DAC Table 4-19 | 48 | 24bit、SNR/DR 109dB |
| 充电模块电气 Table 4-20 | 48 | die 共用内置功能；3.3V 封装未引出 VBUS/VBAT，不接充电 |
| IO 驱动能力 Table 4-21 | 48 | DS1/DS0 → 2/4/8/12mA |
| GPADC 特性 Table 4-22 | 49 | 12-bit |
| 管脚布局 / 管脚描述 | 50–51 | |
| GPIO(PA) 列表 Table 5-2 | 52、54 | |
| 专用管脚 Table 5-3（QFN68） | 61 | 3.3V 系列权威引脚表 |
| 专用管脚 Table 5-4（QFN80） | 62 | |
| 封装尺寸 / 回流焊 | 64–76 | QFN80 尺寸、MSL、回流曲线 |
| 硬件最小系统原理图 | 77 | 图 5-16（矢量图，文本反序） |
| 订购信息 Table 6-1 | 83 | 5 个料号 |

## 实测功耗（V0.2.2 新增，Table 4-5 / 4-6）
**处理器功耗 Table 4-5（@3.8V 供电，CoreMark 负载；关机 2.8µA）**
| 时钟频率 | CoreMark 电流(µA) | 电流增量(µA/MHz) |
|---|---|---|
| 240MHz | 9989 | 37.88 |
| 216MHz | 9080 | — |
| 192MHz | 8160 | 37 |
| 168MHz | 7270 | — |
| 144MHz | 5321 | 32 |
| 120MHz | 4550 | — |
| 48MHz | 1741 | 28 |
| 24MHz | 955 | 25 |
| 12MHz | 658 | — |
| 关机 | 2.8 | — |

> WhileLoop 负载参考：240MHz 8184µA、144MHz 4426µA、48MHz 1535µA、24MHz 870µA、12MHz 611µA。

**BT/BLE 功耗 Table 4-6（@3.3V 供电，单位 µA；Deepsleep 28µA）**
| 模式 | 条件 | 0dBm | 4dBm | 10dBm |
|---|---|---|---|---|
| BT Sniff | 50ms(attempt=4) | 177.9 | 193.7 | 226.6 |
| BT Sniff | 100ms | 88.9 | 96.8 | 113.2 |
| BT Sniff | 200ms | 44.4 | 48.4 | 56.6 |
| BT Sniff | 500ms | 17.8 | 19.3 | 22.6 |
| BT Sniff | 1s | 8.9 | 9.7 | 11.3 |
| BLE ADV | 50ms | 116.8 | 178.6 | 293.8 |
| BLE ADV | 100ms | 66.0 | 91.9 | 146.8 |
| BLE ADV | 200ms | 33.1 | 46.6 | 77.8 |
| BLE ADV | 500ms | 13.3 | 18.8 | 31.9 |
| BLE ADV | 1s | 6.7 | 9.4 | 15.8 |
| BLE Connection | 50ms | 78.5 | 91.3 | 110.6 |
| BLE Connection | 100ms | 41.3 | 48.1 | 55.8 |
| BLE Connection | 200ms | 20.7 | 24.1 | 29.0 |
| BLE Connection | 500ms | 8.3 | 9.7 | 12.6 |
| BLE Connection | 1s | 5.6 | 6.3 | 7.3 |
| Both Scan | Inquiry+Page Scan | 76 | 80.3 | 81.3 |
| Deepsleep | — | 28 | 28 | 28 |

> 注：3.3V 系列的 BT/BLE 功耗以 **3.3V** 测得（即本表数值）；处理器功耗以 **3.8V** 测得（Table 4-5）。两系列测试电压交叉：电池系列 BT/BLE 以 3.8V 测得、处理器以 3.3V 测得，横向比较需注意。

## 订购信息（Table 6-1，P83）
| 料号 | 封装 | 合封规格 | 供电要求 | 每盘 |
|---|---|---|---|---|
| SF32LB57ZUN3N6 | QFN68 7×7-T0.9-P0.35 | 8Mb NOR Flash | 3.3V 供电 | 3000 |
| SF32BPRTUN3N6 | QFN68 7×7-T0.9-P0.35 | 8Mb NOR Flash | 3.3V 供电 | 3000 |
| SF32LB57EYBBN6 | QFN80 8×8-T0.9-P0.35 | 32Mb×2 OPI-PSRAM | 3.3V 供电 | 3000 |
| SF32LB57GYD7N6 | QFN80 8×8-T0.9-P0.35 | 128Mb OPI-PSRAM + 128Mb NOR Flash | 3.3V 供电 | 3000 |
| SF32LB57GVD776 | BGA112 5.4×4.4-T0.98-P0.4 | 128Mb OPI-PSRAM + 128Mb NOR Flash + 128Mb NOR Flash | 3.3V 供电 | 3000 |

> 📌 命名说明：本册料号封装位 **U=QFN68、Y=QFN80、V=BGA112**；末位 **N6** 为工业级（‑40~+85°C，本册唯一温度等级）。`SF32BPRTUN3N6` 与同表 `SF32LB57ZUN3N6` **整行完全一致**（同 QFN68 7×7-T0.9、同 8Mb NOR Flash、同 3.3V、同 3000/reel），为同一器件的**特殊/替代订货型号**（前缀 BPRT 与 LB57Z 等价，非文档笔误，用户确认）。

## 关联页面
- 芯片实体（3.3V 系列）：[[SF32LB57X]]｜家族总览：[[SF32LB57]]｜电池系列规格书：[[SF32LB57x_batt_Datasheet_V0.2.2]]
- 电源外设（双系列差异）：[[SF32LB57_Peripheral_Power]]
- 其余外设：[[SF32LB57_Peripheral_GPIO]]、[[SF32LB57_Peripheral_Clock]]、[[SF32LB57_Peripheral_GPADC]]、[[SF32LB57_Peripheral_Audio]]、[[SF32LB57_Peripheral_BLE_RF]]、[[SF32LB57_Peripheral_Serial]]、[[SF32LB57_Peripheral_Memory]]、[[SF32LB57_Peripheral_Timer_PWM]]、[[SF32LB57_Peripheral_Display]]、[[SF32LB57_Peripheral_USB]]、[[SF32LB57_Peripheral_TSEN]]、[[SF32LB57_Peripheral_Security]]
- 历史版本：[[SF32LB57X_Datasheet_V0.2_20260909]]

## 原始来源完整引用
1. [DS5702-SF32LB57X-芯片技术规格书-V0p2p2-20261102.pdf]，Raw：`raw/mcu/all_chip/datasheets/DS5702-SF32LB57X-芯片技术规格书-V0p2p2-20261102.pdf`，84 页。

## 待核实问题
1. 功耗数据已在 V0.2.2 补充（Table 4-5/4-6 由 TBD 变为实测），但**仍为草稿**，量产前须复核；3.3V 系列处理器功耗以 3.8V 测得，与电池系列（@3.3V）测试电压不同，横向比较需注意。
2. ~~`SF32BPRTUN3N6` 料号命名疑误~~ → 已确认 `SF32BPRTUN3N6` ≡ `SF32LB57ZUN3N6`（同配置的特殊/替代订货型号，非笔误，见订购信息表注），本条闭环。
3. 复位 / BOOT 外部电路、PCB 布局与 EMC/ESD 规范仍缺失；仍待补 SiFli 硬件设计指南（HDG）/参考设计（RF 设计）。
4. QFN68 GPIO 数 = 47（DS Table 5-3，以 DS 为准；UM Table 5-1 / pinmux 显示 12 引脚未引出推得 46，偏差保留待原厂复核 pinmux 表）。
