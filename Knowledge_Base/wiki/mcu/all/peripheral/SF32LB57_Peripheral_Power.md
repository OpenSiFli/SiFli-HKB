---
title: SF32LB57 电源架构与供电外设
type: mcu_peripheral
aliases: ["SF32LB57_Peripheral_Power","SF32LB57电源","SF32LB57 电源架构"]
tags: ["企业MCU","外设-电源","芯片系列-SF32LB57","原理图审图/故障分析/报告输出"]
sources: [[SF32LB57X_Datasheet_V0.2_20260909],[SF32LB57x_batt_Datasheet_V0.2_20260909],[SF32LB57X_Datasheet_V0.2.2],[SF32LB57x_batt_Datasheet_V0.2.2],[UM5701-SF32LB57x-用户手册_V0p1]]
mcu_family: ["SF32LB57"]
series_applicability: ["系列相关-见下表"]
mcu_model: ["SF32LB57X","SF32LB57x"]
hardware_version: ["QFN68 7x7","QFN80 8x8","BGA112 5.4x4.4","Datasheet V0.2","Datasheet V0.2.2"]
created: 2026-09-09
updated: 2026-09-09
---

## 摘要
SF32LB57 家族**两个系列的差异集中在电源架构与电源专用引脚**。本页为双系列电源对照的权威页：3.3V 系列 [[SF32LB57X]] 由外部 PVDD 供电；电池系列 [[SF32LB57x_batt|SF32LB57x]] 由电池经 VCC 供电并内置充电模块。

> ⚠️ 两份规格书均为 **V0.2.2 草稿**（2026-09-24，由 V0.2 演进）：`Table 4-5 处理器功耗`、`Table 4-6 BT/BLE 功耗` **已于 V0.2.2 补充实测值（不再 TBD）**，详见 §8；草稿参数量产前仍须复核。

## 1. 双系列电源引脚对照（QFN68 同封装）
| 引脚 | 3.3V 系列 SF32LB57X | 电池系列 SF32LB57x |
|---|---|---|
| 14 | **AVDD33**（模拟电源输入，接 3.3V） | **VDD33_VOUT2**（3.3V LDO2 输出，150mA） |
| 19 | **PVDD**（PMU 电源输入，接 3.3V） | **VSYS**（电源输出 3.0V，外挂电容到地） |
| 20 | **VBATS**（电池电压检测输入） | **VBAT**（电源输出，接电池；内含 ADC 检测） |
| 21 | **VDDIOA**（GPIO 电源输入） | **VBUS**（内部充电模块电源输入） |
| 22 | **NC**（无信号，悬空） | **VDD33_VOUT1**（3.3V LDO1 输出，150mA） |
| 24 | GPIO（非专用脚） | **VCC**（系统电源输入，接电池） |
| 52 | **AVDD_BRF**（接 3.3V） | **AVDD_BRF**（**接 VSYS**） |

**其余电源相关引脚两系列一致（QFN68）**
| 引脚 | 名称 | 类型 | 说明 |
|---|---|---|---|
| 12 | VDD_RTC | PWR | RTC LDO 输出，内部电源，接电容到地 |
| 13 | VDD_RET | PWR | RET LDO 输出，内部电源，接电容到地 |
| 15 | VDD_VOUT2 | PWR | 内部 LDO2 输出，接电容到地 |
| 16 | VDD_VOUT1 | PWR | 内部 LDO1 输出，接电容到地 |
| 17 | BUCK_FB | PWR | Buck 输出，接电感另一端并接电容到地 |
| 18 | BUCK_LX | A,I/O | Buck 电感开关，接电感一端 |
| 23 | VDD18_VOUT | PWR | 1.8V LDO 输出，内部电源，接电容到地 |
| 38 | AVDD33_AUD | PWR | 音频电源输入，接 3.3V 电源 |
| 39 | AUD_VREF | A,I | 音频参考电压输入，接电容到地，**不用外供电源** |
| 68 | GPADC_VREF | A,I | GPADC VREF 输入，接电容到地，**不用外供电源** |
| 69 | EPAD | GND | 地（散热裸焊盘） |

**QFN80（仅 3.3V 系列）额外/差异引脚**
VDDIOA=4、VBATS=5、PVDD=3、NC=2、BUCK_LX=1、BUCK_FB=80、VDD_VOUT1=79、VDD_VOUT2=78、VDD_RET=76、VDD_RTC=75、**VDDIOSB=6（SIP SB IO 电源输入）**、VDD18_VOUT=7、AVDD33=77、GPADC_VREF=63、AVDD_BRF=41、AVDD33_AUD=22、AUD_VREF=23、EPAD=81。

## 2. 3.3V 系列电源架构（SF32LB57X）
- 主电：**PVDD(19) 接外部 3.3V**（范围 2.97~3.63V，绝对最大 3.63V，已按 DS Table 4-1 为准；UM §3.2 所述「PVDD 支持 1.8V 至 3.3V」不作数）。
- GPIO 域：**VDDIOA(21)** 独立供电。
- 模拟/射频/音频电源：**AVDD33(14)、AVDD_BRF(52)、AVDD33_AUD(38)** 均接外部 3.3V。
- 内部电源链：PVDD → **Buck**（BUCK_LX 18 / BUCK_FB 17，外接电感+电容，**不可旁路**）→ 内部 LDO 出 VDD_VOUT1(16)/VDD_VOUT2(15)、VDD_RET(13)、VDD_RTC(12)、VDD18_VOUT(23)。
- **提供两路对外 3.3V 供电，每路最大 150mA**（对应电池系列的 VDD33_VOUT1/2；3.3V 系列由片内 LDO 提供，具体输出引脚以引脚表为准）。
- **无充电电路**，不可直接接锂电池。

## 3. 电池系列电源架构（SF32LB57x）
- 电池接 **VCC(24)**（系统电源输入）与 **VBAT(20)**（电源输出/电池电压 ADC 检测）。
- 充电输入 **VBUS(21)**（4.5~5.5V）→ 内部充电模块 → VBAT。
- **VSYS(19)** 为 3.0V 输出，**须外挂电容到地**。
- **AVDD_BRF(52) 接 VSYS**（电池系列射频电源来自 VSYS，而非 3.3V）。
- 对外 3.3V：**VDD33_VOUT1(22)** 与 **VDD33_VOUT2(14)**，各 150mA，接电容到地。
- **QFN68 无独立 VDDIOA 外部引脚、无 AVDD33 外部输入、无 PVDD**（IO 由 VSYS 内部供）；⚠️ **QFN80/BGA112 版本（UM §3.1 Table 3-3）则有 VDDIOA（1.8/2.5/3.3V）、AVDD33(AVDD33_ANA)、VBATS(QFN80) 等外部引脚**——「无 VDDIOA」仅限 QFN68。

## 4. 电气参数（V0.2.2 起两册 Table 4-1/4-2 已按封装实际供电精简）
> ⚠️ **V0.2.2 变更**：3.3V 册（DS5702）Table 4-1/4-2 **仅列 PVDD**；电池册（DS5701）Table 4-1/4-2 **仅列 VBUS/VBAT/VCC**。V0.2 曾两册都列全部供电参数（`die 双供电`解读），V0.2.2 已按各封装实际引出引脚清理。裸 die 仍同时支持两种供电（UM §3.1/§3.2 确认），但**布线以引脚表（封装管脚定义）为准**。

**运行条件（DS5702 3.3V 册 V0.2.2 Table 4-1，PDF p37）**
| Symbol | Description | Min | Typ | Max | Unit |
|---|---|---|---|---|---|
| PVDD | System power input | 2.97 | | 3.63 | V |
| Tamb | Ambient temperature | ‑40 | | 85 | °C |
| VIL | CMOS low level input | 0 | | 0.3×VIO | V |
| VIH | CMOS high level input | 0.7×VIO | | VIO | V |
| VTH | CMOS threshold | | 0.5×VIO | | V |

**运行条件（DS5701 电池册 V0.2.2 Table 4-1，PDF p38）**
| Symbol | Description | Min | Typ | Max | Unit |
|---|---|---|---|---|---|
| VBUS | VBUS power input | 4.5 | 5 | 5.5 | V |
| VBAT | VBAT power output | 3.0 | | 4.7 | V |
| VCC | System power input | 3.0* | | 4.7 | V |
| Tamb | Ambient temperature | ‑40 | | 85 | °C |
| VIL | CMOS low level input | 0 | | 0.3×VIO | V |
| VIH | CMOS high level input | 0.7×VIO | | VIO | V |
| VTH | CMOS threshold | | 0.5×VIO | | V |
> * 开机电压 3.5V ±10%

**绝对最大值（V0.2.2 Table 4-2）**
- 3.3V 册：PVDD 3.63V；Tstorage ‑40~125°C
- 电池册：VBUS 5.5V；VBAT 4.7V；VCC 4.7V；Tstorage ‑40~125°C

## 5. 充电模块（电池系列，Table 4-20）
| 参数 | 符号 | Min | Typ | Max | 单位 |
|---|---|---|---|---|---|
| 充电输入电压 | Vbus | 4.5 | 5 | 5.5 | V |
| BAT 漏电流 | Ileak | | | 30 | nA |
| 恒流充电电流 | Icc | 5 | | 400 | mA |
| 涓流充电电流 | Itri | 0.5 | 30 | 40 | mA |
| 满电截止电流比 | Iend | | 10 | | % of Icc |
| 满电截止电压 | Vcv | 4.2 | | 4.45 | V |
| 满电电压精度 | | | | ±1.5 | % of Vcv |
| 涓流→恒流阈值 | Vcc | | 3 | | V |
| 复充阈值 | Vrechg | | Vcv‑0.15 | | V |
| 过充保护 | Vhigh | | | 4.78 | V |
> ⚠️ **说明**：充电模块为芯片裸 die 共用的内置功能，故该表同时出现在 3.3V 规格书中属正常（非误植）。但 QFN68 3.3V 封装未将 VBUS/VBAT/VCC 引出到封装引脚（pin19/21/22 = PVDD/VDDIOA/NC），故 3.3V 版本原理图无需连接充电电路；电池系列按 DS5701 Table 5-3 连接 VBUS(21)/VBAT(20)/VCC(24)。

## 6. 硬件设计约束
- **3.3V 系列**：PVDD/AVDD33/AVDD33_AUD/AVDD_BRF 均不得超过 **3.63V**。
- **电池系列**：VBUS ≤5.5V，VBAT/VCC ≤4.7V；过充保护 4.78V。
- **BUCK 不可旁路**：BUCK_LX 外接电感、BUCK_FB 外接电容到地。
- **VDD_VOUT1/2、VDD_RET、VDD_RTC、VDD18_VOUT、VDD33_VOUT1/2 均为电源输出**，仅需外接电容，**禁止外部供电**。
- **GPADC_VREF(68)、AUD_VREF(39)** 接电容到地，**不用外供电源**。
- **NC(22，3.3V 系列) 必须悬空**。
- **EPAD(69) 必须可靠接地**。
- **VDD33_VOUT1/2 各带载 ≤150mA**（电池系列）。
- **电池系列 AVDD_BRF 接 VSYS**，不可接 3.3V。

## 7. UM5701 V0.1 补充（寄存器级手册）
- **PMUC 电源管理寄存器（§3.4，印刷 p34–35）**：WER[30] **PWRKEY = 1**（reset）使能 PA34 长高重启整芯片；置 **0** 可关闭（PA34 即可当普通 GPIO 无时长限制，见 [[DR_PA34_PowerKey_GPIO]]）。WER[1] PA34 作唤醒源；WER[13:10] PA24–PA27、WER[29] LOWBAT 等作唤醒源。
- **电源树细化（§3.1/§3.2）**：VCC→VSYS(3.3V LDO)→BUCK→BUCK_FB(1.25V)→VDD_VOUT1(1.1V HPSYS)/VDD_VOUT2(0.9V LPSYS)/VDD_RET(0.7V)；VDD33_VOUT1/2(3.3V LDO2/3，BOOTROM 自启 VOUT1)、VDD18_VOUT(1.8V 供 SIP PSRAM)、VDD_RTC(1.1V，hibernate 保持)；BUCK 不可旁路（§3.2 明确）。
- **充电模块**：最大恒流 **400mA**、Vcv 4.2~4.45V（与 DS 一致，UM §3.3）。
- **Hibernate 合封供电**：外部供电的合封存储应由 **PA21** 控电源开关（QFN68 未引出 → 改用 PA11，见 [[CR_Hibernate_SiP_Supply]]，封装依赖非系列依赖）。


## 8. 实测功耗（V0.2.2 新增，Table 4-5 / 4-6）
> V0.2.2（2026-09-24）补充了 V0.2 全 TBD 的处理器与 BT/BLE 功耗实测值。注意两系列测试电压不同：3.3V 系列处理器 @3.8V、BT/BLE @3.3V；电池系列处理器 @3.3V、BT/BLE @3.8V。

**3.3V 系列（DS5702 Table 4-5 @3.8V，CoreMark 负载；关机 2.8µA）**
| 时钟 | CoreMark 电流(µA) | 增量(µA/MHz) |
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

**电池系列（DS5701 Table 4-5 @3.3V，CoreMark 负载；关机 1.0µA）**
| 时钟 | CoreMark 电流(µA) | 增量(µA/MHz) |
|---|---|---|
| 240MHz | 11554 | 45 |
| 216MHz | 10471 | — |
| 192MHz | 9408 | 45 |
| 168MHz | 8333 | — |
| 144MHz | 5841 | 36 |
| 120MHz | 4970 | — |
| 48MHz | 1823 | 31 |
| 24MHz | 860 | 26 |
| 12MHz | 546 | — |
| 关机 | 1.0 | — |

**BT/BLE 功耗 — 3.3V 系列（DS5702 Table 4-6 @3.3V，单位 µA；Deepsleep 28µA）**
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

**BT/BLE 功耗 — 电池系列（DS5701 Table 4-6 @3.8V，单位 µA；Deepsleep 28µA）**
| 模式 | 条件 | 0dBm | 4dBm | 10dBm |
|---|---|---|---|---|
| BT Sniff | 50ms(attempt=4) | 170.5 | 178.5 | 212.3 |
| BT Sniff | 100ms | 85.1 | 89.2 | 106.1 |
| BT Sniff | 200ms | 42.5 | 44.6 | 53.0 |
| BT Sniff | 500ms | 17.0 | 17.8 | 21.2 |
| BT Sniff | 1s | 8.5 | 8.9 | 10.6 |
| BLE ADV | 50ms | 134.3 | 161.5 | 263.4 |
| BLE ADV | 100ms | 70.7 | 83.1 | 148.7 |
| BLE ADV | 200ms | 35.7 | 42.2 | 74.4 |
| BLE ADV | 500ms | 14.4 | 17.0 | 30.5 |
| BLE ADV | 1s | 7.2 | 8.5 | 15.4 |
| BLE Connection | 50ms | 81.4 | 83.0 | 105.1 |
| BLE Connection | 100ms | 41.2 | 45.8 | 52.6 |
| BLE Connection | 200ms | 21.3 | 22.9 | 27.8 |
| BLE Connection | 500ms | 9.2 | 9.2 | 12.0 |
| BLE Connection | 1s | 5.5 | 5.7 | 6.8 |
| Both Scan | Inquiry+Page Scan | 67.8 | 68.9 | 68.4 |
| Deepsleep | — | 28 | 28 | 28 |

> 休眠(Deepsleep) 28µA（两系列一致）；特性页旧宣称 3.3V 系列 1µA@3.3V、电池系列 2.8µA@3.8V 与表 4-6 实测 28µA 不符，以表为准。
## 关联页面
- 3.3V 实体：[[SF32LB57X]]｜电池实体：[[SF32LB57x_batt|SF32LB57x]]｜家族：[[SF32LB57]]
- 来源：[[SF32LB57X_Datasheet_V0.2.2]]、[[SF32LB57x_batt_Datasheet_V0.2.2]]（V0.2.2，最新）、[[SF32LB57X_Datasheet_V0.2_20260909]]、[[SF32LB57x_batt_Datasheet_V0.2_20260909]]（V0.2，历史）、[[UM5701-SF32LB57x-用户手册_V0p1]]
- 相关：[[SF32LB57_Peripheral_GPADC]]、[[SF32LB57_Peripheral_Audio]]、[[SF32LB57_Peripheral_Clock]]

## 原始来源完整引用
1. [DS5702-SF32LB57X V0.2]，Raw：`raw/mcu/all_chip/datasheets/DS5702-SF32LB57X-芯片技术规格书-V0p2-20260909.pdf`，Table 4-1~4-3（P39）、Table 4-20/4-21（P48）、Table 5-3（P61）、Table 5-4（P62）。
2. [DS5702-SF32LB57X V0.2.2]，Raw：`raw/mcu/all_chip/datasheets/DS5702-SF32LB57X-芯片技术规格书-V0p2p2-20261102.pdf`，84 页；Table 4-1/4-2 精简为仅 PVDD、Table 4-5/4-6 新增功耗实测。
2. [DS5701-SF32LB57x V0.2]，Raw：`raw/mcu/all_chip/datasheets/DS5701-SF32LB57x-芯片技术规格书-V0p2-20260909.pdf`，Table 4-1~4-3（P40）、Table 4-20/4-21（P49）、Table 5-3（P62）。
3. [UM5701-SF32LB57x-CN V0.1]，第 3 章 电源管理（印刷 p27–35，PDF p42–50）：§3.1/§3.2 双系列电源结构图与电源管脚表、§3.3 充电模块、§3.4 PMUC 寄存器。

## 待核实问题
1. 3.3V 系列是否具备对外 3.3V 输出引脚（特性页称"提供两路对外 3.3V 供电，每路最大 150mA"，但 QFN68 引脚表未标明对应输出脚），待确认。
2. 功耗参数已于 V0.2.2 补充实测（DS V0.2.2 Table 4-5/4-6，见 §8）；草稿值量产前仍须复核。
3. V0.2.2 已按封装实际供电精简两册 Table 4-1/4-2（3.3V 册仅 PVDD、电池册仅 VBUS/VBAT/VCC）；裸 die 双供电能力由 UM §3.1/§3.2 确认，布线以引脚表为准；待原厂最终确认。
4. **PVDD 电压范围冲突已按 DS 闭环**：UM §3.2 正文「PVDD 支持 1.8V 至 3.3V」与 DS Table 4-1（2.97~3.63V）不一致；按用户判定 **以 DS 为准 = 2.97~3.63V**，UM 该表述不作数。
5. **电池系列 QFN80/BGA112 电源引脚**：UM §3.1 Table 3-3 确认有 VDDIOA/AVDD33/VBATS 等外部引脚（仅 QFN68 无 VDDIOA），「无 VDDIOA」仅限 QFN68。
