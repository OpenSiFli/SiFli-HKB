---
title: SF32LB57X
type: mcu_chip
aliases: ["SF32LB57X 3.3V系列","SF32LB57X(3.3V供电)","SiFli SF32LB57X"]
tags: ["企业MCU","芯片系列-SF32LB57","型号-SF32LB57X 3.3V系列","芯片实体","原理图审图/故障分析/报告输出"]
sources: [[SF32LB57X_Datasheet_V0.2_20260909],[SF32LB57X_Datasheet_V0.2.2],[SF32LB57x_batt_Datasheet_V0.2.2],[UM5701-SF32LB57x-用户手册_V0p1]]
mcu_family: ["SF32LB57"]
mcu_series: ["3.3V供电系列"]
mcu_model: ["SF32LB57ZUN3N6","SF32BPRTUN3N6","SF32LB57EYBBN6","SF32LB57GYD7N6","SF32LB57GVD776"]
hardware_version: ["QFN68 7x7","QFN80 8x8","BGA112 5.4x4.4","Datasheet V0.2","Datasheet V0.2.2","UM V0.1"]
created: 2026-09-09
updated: 2026-11-02
---

## 摘要
SiFli **SF32LB57X（大写 X）= 3.3V 供电系列**，超低功耗多核双系统 MCU（双 Arm Cortex-M33 STAR-MC1 @240MHz / 24MHz，1968 CoreMark），集成 2D/2.5D ePicasso 3.0 图形引擎、592KB SRAM、双模蓝牙 6.3、BT/BLE Audio。**无充电电路**，主电由外部 **PVDD（3.3V）** 提供，GPIO 由 **VDDIOA** 供电。适用于外部 3.3V 稳压供电的工业传感、智能 HMI、网关、显示类设备。

> 同家族另有 **SF32LB57x（小写 x）= 电池供电系列**（内置充电模块、VBUS/VBAT/VCC/VSYS），见 [[SF32LB57x_batt|SF32LB57x]]；家族总览见 [[SF32LB57]]。
>
> 📌 **文档来源**：本系列由 **DS5702-SF32LB57X-CN**（标题大写 X）描述，与芯片命名一致。最新版见 [[SF32LB57X_Datasheet_V0.2.2]]（V0.2.2）；历史版 [[SF32LB57X_Datasheet_V0.2_20260909]]（V0.2）。
>
> ⚠️ **本系列规格书为 V0.2.2 草稿**（2026-09-24，由 V0.2 演进，已补充功耗实测数据），草稿参数量产前仍须复核。

## 核心硬件内容

### 1. 芯片基础
- 厂商 / 系列：SiFli（思澈科技）/ SF32LB57X 3.3V 供电系列（57 家族）
- 内核：双 Arm Cortex-M33 STAR-MC1（HPSYS 双性能核）@240MHz，1968 CoreMark
  - HCPU：32KB(2-way) I$ + 16KB(4-way) D$
  - ACPU：16KB(2-way) I$ + 8KB(4-way) D$
  - LPSYS：Cortex-M33 STAR-MC1 @24MHz
- 存储：片上 SRAM **592KB**（HPSYS 512KB + LPSYS 80KB，全部 Retention）；SiP 合封随料号（见 §订购信息）
- FPU（单精度）、MPU；功耗效率 23µA/MHz @3.8V
- 封装与 GPIO：
  - **QFN68** 7×7mm（T0.9 / T1.1，P0.35）— 3.3V 供电，**47 GPIO**（以 DS Table 5-3 为准；UM Table 5-1 / pinmux 显示 12 引脚未引出，与 DS 偏差，按 DS 为准）
  - **QFN80** 8×8mm（T0.9 / T1.1，P0.35）— 3.3V 供电，**58 GPIO**
  - **BGA112** 5.4×4.4mm（T0.98，P0.4）— 3.3V 供电，**最多 58 GPIO + 6 GPO MPI IO**
- 工作温度：‑40 ~ +85 °C
- 无线：双模蓝牙 6.3（BLE Audio / LE LR）；BLE 灵敏度 ‑100dBm(1Mbps)；最大发射 17dBm(BR/BLE)、13dBm(EDR2/3)
- 电源管理：内置高效率 Buck + 低功耗 LDO；**提供两路对外 3.3V 供电，每路最大 150mA**；休眠 **Deepsleep 28µA**（V0.2.2 Table 4-6 实测；特性页旧宣称 1µA@3.3V 与表不符）
- **无锂电充电电路**（主电由外部 PVDD 提供；VBATS 仅作电池电压检测）

### 2. 专用引脚（Table 5-3，QFN68 = 57XUxxx6）
| 引脚 | 名称 | 类型 | 说明 |
|---|---|---|---|
| 21 | VDDIOA | PWR | GPIO 电源输入（独立 IO 域） |
| 20 | VBATS | A,I | 电池电压检测输入 |
| 19 | PVDD | PWR | PMU 电源输入，**接 3.3V 电源** |
| 18 | BUCK_LX | A,I/O | Buck 电感开关，接电感一端 |
| 17 | BUCK_FB | PWR | Buck 输出，接电感另一端并接电容到地 |
| 16 | VDD_VOUT1 | PWR | 内部 LDO1 输出，接电容到地 |
| 15 | VDD_VOUT2 | PWR | 内部 LDO2 输出，接电容到地 |
| 13 | VDD_RET | PWR | RET LDO 输出，接电容到地 |
| 12 | VDD_RTC | PWR | RTC LDO 输出，接电容到地 |
| 23 | VDD18_VOUT | PWR | 1.8V LDO 输出，接电容到地 |
| 14 | AVDD33 | PWR | 模拟电源输入，**接 3.3V 电源** |
| 68 | GPADC_VREF | A,I | GPADC VREF 输入，接电容到地，**不用外供电源** |
| 55 / 54 | XTAL48M_XI / XO | A,I / A,O | 48MHz 晶体接口 |
| 53 | BRF_ANT | A,I/O | 天线接口 |
| 52 | AVDD_BRF | PWR | 射频电源输入，**接 3.3V 电源** |
| 39 | AUD_VREF | A,I | 音频参考电压输入，接电容到地，不用外供电源 |
| 38 | AVDD33_AUD | PWR | 音频电源输入，接 3.3V 电源 |
| 37 / 36 | DACP / DACN | A,O | 音频 DAC 输出 |
| 22 | NC | — | 无信号（悬空） |
| 69 | EPAD | GND | 散热裸焊盘，须接 GND |

**QFN80（Table 5-4，57XYxxx6）关键差异**：VDDIOA=4、VBATS=5、PVDD=3、NC=2、BUCK_LX=1、BUCK_FB=80、VDD_VOUT1=79、VDD_VOUT2=78、VDD_RET=76、VDD_RTC=75、**VDDIOSB=6（SIP SB IO 电源输入）**、VDD18_VOUT=7、AVDD33=77、GPADC_VREF=63、XTAL48M_XI/XO=44/43、BRF_ANT=42、AVDD_BRF=41、AUD_VREF=23、AVDD33_AUD=22、DACP/DACN=21/20、EPAD=81。

### 3. 极限电气参数（Table 4-2）
- **PVDD**：最大 **3.63V**
- **VBUS/VBAT/VCC 不在本系列规格书 Table 4-1/4-2 中**（V0.2.2 已精简为仅 PVDD）；其为电池系列供电，3.3V 封装未引出对应引脚（pin19/21/22 = PVDD/VDDIOA/NC），不可作为 3.3V 供电输入。裸 die 双供电能力见 [[SF32LB57x_batt|SF32LB57x]] / UM §3.1/§3.2。
- 存储温度 Tstorage：‑40 ~ +125 °C
- ESD：**HBM ±4000V**（JS-001-2017）、**CDM ±1000V**（JS-002-2018）
- Latch-up：I-Test ±200mA、OVT +1.5×VddMAX（JESD78E）
- MSL：**MSL3**（125°C 烘烤 24h；30°C/60%RH 浸泡 192h；回流 260+0°C 20s ×3）
- 可靠性：HTOL Tj=125°C 1000h；TCT ‑65~150°C 1000 次；uHAST 130°C/85%RH 96h；HTSL 150°C 1000h；PCT 121°C/100%RH/205kPa 96h；可焊性 245°C

### 4. 推荐工作条件（DS5702 V0.2.2 Table 4-1，PDF p37）
- **PVDD**：2.97 ~ 3.63 V（已按 DS Table 4-1 为准；UM §3.2 所述「PVDD 支持 1.8V 至 3.3V」不作数）
- Tamb：‑40 ~ +85 °C
- VIL：0 ~ 0.3×VIO；VIH：0.7×VIO ~ VIO；VTH：0.5×VIO
- ⚠️ V0.2.2 起 3.3V 册 Table 4-1/4-2 **仅列 PVDD**（不再含 VBUS/VBAT/VCC）；VBUS/VBAT/VCC 为电池系列供电（见 [[SF32LB57x_batt|SF32LB57x]]），3.3V 封装未引出对应引脚。

### 5. I/O 特性（Table 4-3 @3.3V）
- 管脚电容 CIN：2.5 ~ 3 ~ 3.5 pF
- VIH ≥ 0.7×VDD；VIL ≤ 0.3×VDD
- IIH / IIL：10 ~ 40 nA
- VOH ≥ 0.8×VDD（高阻负载）；VOL ≤ 0.2×VDD
- **驱动电流 IOH / IOL：12 / 30 / 38 mA**（min/typ/max，最大驱动强度）
- 内部上拉 RPU / 下拉 RPD：7 / 10 / 20 kΩ
- 复位：VIH_nRST ≥ 0.7×VDD；VIL_nRST ≤ 0.3×VDD
- **IO 驱动能力档位（Table 4-21）**：DS1/DS0 = 00→2mA、01→4mA、10→8mA、11→12mA

### 6. 接口速率（Table 3-1）
| 控制器 | 最高速率 | 备注 |
|---|---|---|
| MPI1 | 156 MHz（OPI-PSRAM，DDR）/ 104 MHz（QSPI-NOR，DTR） | 合封 |
| MPI2 | 156 MHz（OPI-PSRAM，DDR）/ 104 MHz（QSPI-NOR，DTR） | 合封 |
| MPI3 | 104 MHz | 外挂 QSPI-NOR / QSPI-NAND Flash（SDR） |
| SDMMC | 48 MHz | 外挂 eMMC |
| I2C | 3.4 MHz | |
| SPI | 48 MHz | |
| UART | 3 Mbaud | |
| I2S | 48 kHz 采样 | 48kHz × 32-bit × 2 channel |
| PDM | 3.072 MHz | |
| GPADC | 4 Msps | |

### 7. 最小系统要点
- **电源**：PVDD(19) 接 3.3V；VDDIOA(21) 供 GPIO；AVDD33(14)/AVDD33_AUD(38)/AVDD_BRF(52) 均接 3.3V；BUCK_FB(17) 不可旁路，BUCK_LX(18) 接电感；VDD_VOUT1/2、VDD_RET、VDD_RTC、VDD18_VOUT 为**内部电源输出，各接电容到地**；GPADC_VREF(68)、AUD_VREF(39) 接电容到地且**不用外供电源**。
- **时钟**：48MHz 晶体接 XTAL48M_XI(55)/XO(54)。
- **EPAD(69)** 必须可靠接 GND；**NC(22)** 悬空。
- 官方最小系统原理图见 DS5702 **P77**（图 5-16：SF32LB57XUxxx6 3.3V 常规供电最小系统原理图）。
- 复位 / BOOT 外部电路：规格书未以可读文本给出，待补用户手册与参考设计。

### 8. 订购信息（Table 6-1，P83）
| 料号 | 封装 | 合封规格 | 供电要求 | 每盘 |
|---|---|---|---|---|
| SF32LB57ZUN3N6 | QFN68 7×7-T0.9-P0.35 | 8Mb NOR Flash | 3.3V 供电 | 3000 |
| SF32BPRTUN3N6 | QFN68 7×7-T0.9-P0.35 | 8Mb NOR Flash | 3.3V 供电 | 3000 |
| SF32LB57EYBBN6 | QFN80 8×8-T0.9-P0.35 | 32Mb×2 OPI-PSRAM | 3.3V 供电 | 3000 |
| SF32LB57GYD7N6 | QFN80 8×8-T0.9-P0.35 | 128Mb OPI-PSRAM + 128Mb NOR Flash | 3.3V 供电 | 3000 |
| SF32LB57GVD776 | BGA112 5.4×4.4-T0.98-P0.4 | 128Mb OPI-PSRAM + 128Mb NOR Flash + 128Mb NOR Flash | 3.3V 供电 | 3000 |
> 📌 `SF32BPRTUN3N6` 与 `SF32LB57ZUN3N6` **整行完全一致**（同 QFN68 7×7-T0.9、同 8Mb NOR Flash、同 3.3V、同 3000/reel），为同一器件的**特殊/替代订货型号**（前缀 BPRT 与 LB57Z 等价，非文档笔误，用户确认）。

### 9. 约束禁忌
- **PVDD/AVDD33/AVDD33_AUD/AVDD_BRF 均不得超过 3.63V**（绝对最大值）。
- **BUCK 不可旁路**；BUCK_LX 外接电感、BUCK_FB 外接电容到地。
- **VDD_VOUT1/2、VDD_RET、VDD_RTC、VDD18_VOUT 为内部电源输出**，仅需外接电容，**禁止外部供电**。
- **GPADC_VREF(68)、AUD_VREF(39) 接电容到地，不用外供电源**。
- **NC(22) 必须悬空**，不可连接。
- **EPAD(69) 必须可靠接地**。
- **BRF_ANT(53)** 片上集成 Balun 与匹配，通常无需片外匹配网络；射频指标基于 **外部 π 型匹配网络**测得（Table 4-7~4-17 注 *）。
- 本系列**无充电电路**，不可直接接锂电池；需电池供电请选 [[SF32LB57x_batt|SF32LB57x]]。

## 关联页面
- 家族总览：[[SF32LB57]]｜电池系列实体：[[SF32LB57x_batt|SF32LB57x]]
- 来源摘要：[[SF32LB57X_Datasheet_V0.2.2]]（V0.2.2，最新）/ [[SF32LB57X_Datasheet_V0.2_20260909]]（V0.2，历史）
- 外设：[[SF32LB57_Peripheral_Power]]、[[SF32LB57_Peripheral_GPIO]]、[[SF32LB57_Peripheral_Clock]]、[[SF32LB57_Peripheral_GPADC]]、[[SF32LB57_Peripheral_Audio]]、[[SF32LB57_Peripheral_BLE_RF]]、[[SF32LB57_Peripheral_Serial]]、[[SF32LB57_Peripheral_Memory]]、[[SF32LB57_Peripheral_Timer_PWM]]、[[SF32LB57_Peripheral_Display]]、[[SF32LB57_Peripheral_USB]]

## 原始来源完整引用
1. [DS5702-SF32LB57X-芯片技术规格书-V0p2-20260909.pdf]，Raw：`raw/mcu/all_chip/datasheets/DS5702-SF32LB57X-芯片技术规格书-V0p2-20260909.pdf`，Table 4-1~4-3（P39）、Table 4-4~4-6（P40–41）、Table 4-18~4-21（P47–48）、Table 3-1（P38）、Table 5-3（P61）、Table 5-4（P62）、Table 6-1（P83）、最小系统图（P77）。
2. [DS5702-SF32LB57X-芯片技术规格书-V0p2p2-20261102.pdf]（V0.2.2），Raw：`raw/mcu/all_chip/datasheets/DS5702-SF32LB57X-芯片技术规格书-V0p2p2-20261102.pdf`，84 页；相对 V0.2 新增功耗实测（Table 4-5/4-6）并精简 Table 4-1/4-2 为仅 PVDD。

## 待核实问题
1. **V0.2.2 已精简供电参数**：3.3V 册 Table 4-1/4-2 现仅列 **PVDD（2.97~3.63V，绝对最大 3.63V）**，不再含 VBUS/VBAT/VCC（V0.2 曾一并列出，V0.2.2 按封装实际供电清理）。VBUS/VBAT/VCC 为电池系列供电，3.3V 封装未引出对应引脚（pin19/21/22 = PVDD/VDDIOA/NC），原理图按引脚表布线、不接充电电路。裸 die 双供电能力见 UM §3.1/§3.2。
2. **Table 4-20 充电模块电气特性出现在本册**属正常（die 共用内置功能），非误植；本系列封装未引出 VBUS/VBAT，无需连接。
3. **功耗参数已于 V0.2.2 补充实测**（Table 4-5 处理器 @3.8V、Table 4-6 BT/BLE @3.3V，Deepsleep 28µA），低功耗预算可初步核算；草稿值量产前须复核（见 [[SF32LB57_Peripheral_Power]]）。
4. ~~`SF32BPRTUN3N6` 料号疑误~~ → 已确认 `SF32BPRTUN3N6` ≡ `SF32LB57ZUN3N6`（同配置的特殊/替代订货型号，非笔误），本条闭环。
5. 复位 / BOOT 外部电路、PCB 布局与 EMC/ESD 规范缺失——用户手册 UM5701 已导入（寄存器级，无上述硬件设计内容），仍待补 SiFli **硬件设计指南（HDG）/参考设计（RF 设计）**。
6. **QFN68 GPIO 数已按 DS 闭环**：DS Table 5-3 标 47；UM Table 5-1 与 pinmux 显示 12 引脚未引出（PA21 + PA45–PA54 + PA57），与 DS 偏差，按用户判定 **以 DS 为准 = 47**。
