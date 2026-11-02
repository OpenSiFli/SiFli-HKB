---
title: DS5701-SF32LB57x 芯片技术规格书 V0.2.2（2026-09-24）
type: mcu_source
aliases: ["SF32LB57x_batt_Datasheet_V0.2.2","DS5701-SF32LB57x-CN","SF32LB57x规格书V0.2.2"]
tags: ["企业MCU","来源文档","规格书/datasheet","芯片系列-SF32LB57","电池供电系列"]
doc_type: datasheet
doc_version: V0.2.2
doc_date: 2026-09-24
doc_status: 草稿（0.0.0~0.9.9，非正式发布，参数随时可能更改）
vendor: 思澈科技（南京）有限公司 SiFli
sources: ["[[raw/mcu/all_chip/datasheets/DS5701-SF32LB57x-芯片技术规格书-V0p2p2-20261102.pdf]]"]
mcu_family: ["SF32LB57"]
mcu_series: ["电池供电系列"]
mcu_model: ["SF32LB57x"]
hardware_version: ["QFN68 7x7","Datasheet V0.2.2"]
created: 2026-11-02
updated: 2026-11-02
---

## 摘要
SiFli **SF32LB57x（小写 x）= 电池供电系列** 芯片技术规格书。文档编号 **DS5701-SF32LB57x-CN**，版本 **V0.2.2**（2026-09-24 更新），PDF 共 85 页。

> 📌 **系列判定**：`DS5701-SF32LB57x`（小写 x）= 电池系列；3.3V 系列为 `DS5702-SF32LB57X`（大写 X）。
>
> 📌 **NTFS 命名说明**：本页文件名带 `_batt` 后缀，因 Windows/NTFS 不区分大小写，`SF32LB57X_*.md` 与 `SF32LB57x_*.md` 会冲突。引用本页请写 `[[SF32LB57x_batt_Datasheet_V0.2.2|SF32LB57x 规格书]]`。

## 文档状态与版本演进
- 本版为 **草稿（V0.2.2）**：0.0.0~0.9.9 为初稿，信息为初步数据，**不能保证准确性，随时可能更改，不主动通知**。
- **更新历史（PDF p2）**：
  - 2026-05-29 0.1 初稿
  - 2026-08-29 0.2 更新封装信息，IO 信息，蓝牙相关指标
  - 2026-09-16 0.2.1 封装信息勘误
  - **2026-09-24 0.2.2 增加了 CPU 以及蓝牙的功耗数据**（Table 4-5 / Table 4-6 由 TBD 变为实测值）
  - 此外 **Table 4-1 / Table 4-2 已精简**：电池册仅保留本系列实际供电 **VBUS/VBAT/VCC**（不再列 PVDD，PVDD 为 3.3V 系列供电），与 3.3V 册对称清理。

## 电池系列专属要点（本册权威）
依据 **Table 5-3（QFN68，57xUxxx6）专用管脚表**，电池系列电源引脚如下：

| 引脚 | 名称 | 类型 | 说明 |
|---|---|---|---|
| 21 | VBUS | PWR | **内部充电模块的电源输入口** |
| 20 | VBAT | PWR | 电源输出，接电池；内部有 ADC 检测电池电压值 |
| 19 | VSYS | PWR | 电源输出 3.0V，外挂电容到地 |
| 24 | VCC | PWR | 系统电源输入，接电池 |
| 22 | VDD33_VOUT1 | PWR | 3.3V LDO1 输出，接电容到地，带载能力 **150mA** |
| 14 | VDD33_VOUT2 | PWR | 3.3V LDO2 输出，接电容到地，带载能力 **150mA** |
| 23 | VDD18_VOUT | PWR | 1.8V LDO 输出，内部电源，接电容到地 |
| 52 | AVDD_BRF | PWR | 射频电源输入，**接 VSYS** |
| 38 | AVDD33_AUD | PWR | 音频电源输入，接 3.3V 电源 |
| 18/17 | BUCK_LX / BUCK_FB | A,I/O / PWR | Buck 电感开关 / Buck 输出（接电感另一端并接电容到地） |
| 16/15 | VDD_VOUT1 / VDD_VOUT2 | PWR | 内部 LDO1/LDO2 输出，接电容到地 |
| 13/12 | VDD_RET / VDD_RTC | PWR | RET/RTC LDO 输出，接电容到地 |
| 68 | GPADC_VREF | A,I | GPADC VREF 输入，接电容到地，**不用外供电源** |
| 55/54 | XTAL48M_XI / XO | A,I / A,O | 48MHz 晶体接口 |
| 53 | BRF_ANT | A,I/O | 天线接口 |
| 39 | AUD_VREF | A,I | 音频参考电压输入，接电容到地，不用外供电源 |
| 37/36 | DACP / DACN | A,O | 音频 DAC 输出 |
| 69 | EPAD | GND | 地 |

**与 3.3V 系列（[[SF32LB57X]]）的引脚差异（同 QFN68 封装对照）**
| 引脚 | 3.3V 系列 SF32LB57X | 电池系列 SF32LB57x |
|---|---|---|
| 14 | AVDD33（模拟电源输入，接 3.3V） | VDD33_VOUT2（3.3V LDO2 输出，150mA） |
| 19 | PVDD（PMU 电源输入，接 3.3V） | VSYS（电源输出 3.0V） |
| 20 | VBATS（电池电压检测输入） | VBAT（电源输出，接电池） |
| 21 | VDDIOA（GPIO 电源输入） | VBUS（充电模块电源输入） |
| 22 | NC（无信号） | VDD33_VOUT1（3.3V LDO1 输出，150mA） |
| 24 | GPIO（非专用脚） | VCC（系统电源输入，接电池） |
| 52 | AVDD_BRF（接 3.3V） | AVDD_BRF（**接 VSYS**） |

> 结论：两系列**仅电源专用引脚不同**（pin 14/19/20/21/22/24 与 AVDD_BRF 供电源），其余外设/射频/音频/晶体/GPADC/地引脚一致；**GPIO 数由封装决定（QFN68 = 47、QFN80/BGA112 = 58，两系列同封装相同，以 DS Table 5-3 为准）**——旧口径「电池系列因 pin24 被 VCC 占用少 1 个 GPIO」已作废。

## 共享规格（与 3.3V 系列一致，详见 [[SF32LB57X_Datasheet_V0.2.2]]）
双核 Cortex-M33 STAR-MC1 @240MHz/24MHz、1968 CoreMark、592KB SRAM、2D/2.5D ePicasso 3.0、eZip 3.0、BT/BLE 6.3、24-bit 音频 ADC/DAC、3×MPI + 2×SD/SDIO、58 GPIO+6 GPO MPI IO、3×UART/4×I2C/2×SPI/2×I2S/2×PDM/USB2.0 FS/2×CAN、12-bit GPADC×12 通道、‑40~+85°C。

## 关键章节页码索引（V0.2.2 PDF 页；电池册比 3.3V 册多 1 页）
| 内容 | PDF 页 | 备注 |
|---|---|---|
| 更新历史 / 文档状态 | 2 | 草稿声明 + 版本演进 |
| 产品概述 / 功能框图 | 3 | |
| 产品特性 | 4–5 | |
| 电源管理架构（图 1-3） | 19 | 含 VSYS/VBAT/VBUS/VCC 关系 |
| 运行条件 Table 4-1 | 38 | **VBUS 4.5~5.5V、VBAT 3.0~4.7V、VCC 3.0~4.7V**（无 PVDD，V0.2.2 精简） |
| 绝对最大值 Table 4-2 | 38 | **VBUS/VBAT/VCC 4.7V（VBUS 5.5V）、Tstorage ‑40~125°C** |
| I/O 特性 @3.3V Table 4-3 | 38 | IOH/IOL 12/30/38mA |
| 可靠性测试 Table 4-4 | 39 | HBM ±4000V、CDM ±1000V、MSL3 |
| 处理器功耗 Table 4-5 | 40 | ✅ **V0.2.2 新增实测（@3.3V）** |
| BT/BLE 功耗 Table 4-6 | 40 | ✅ **V0.2.2 新增实测（@3.8V）** |
| BLE / 经典蓝牙射频 | 41–47 | |
| 音频 ADC/DAC Table 4-18/4-19 | 47–48 | |
| 充电模块电气 Table 4-20 | 48 | **本系列适用** |
| IO 驱动能力 Table 4-21 | 49 | DS1/DS0 → 2/4/8/12mA |
| GPIO(PA) 列表 Table 5-2 | 53、55 | |
| 专用管脚 Table 5-3（QFN68） | 62 | **电池系列权威引脚表** |
| 专用管脚（次封装） | 63–65 | |
| 封装尺寸 / 回流焊 | 65–76 | |
| 硬件最小系统原理图 | 78–82 | |
| 订购信息 Table 6-1 | 84 | 6 个料号，全部 QFN68 锂电池供电 |

## 实测功耗（V0.2.2 新增，Table 4-5 / 4-6）
**处理器功耗 Table 4-5（@3.3V 供电，CoreMark 负载；关机 1.0µA）**
| 时钟频率 | CoreMark 电流(µA) | 电流增量(µA/MHz) |
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

> WhileLoop 负载参考：240MHz 9483µA、144MHz 4853µA、48MHz 1586µA、24MHz 767µA、12MHz 566µA。

**BT/BLE 功耗 Table 4-6（@3.8V 供电，单位 µA；Deepsleep 28µA）**
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

> 注：电池系列的 BT/BLE 功耗以 **3.8V** 测得（锂电满电电压）；处理器功耗以 **3.3V** 测得。休眠(Deepsleep) 28µA。

## 订购信息（Table 6-1，P84）
| 料号 | 封装 | 合封规格 | 供电要求 | 每盘 |
|---|---|---|---|---|
| SF32LB573UB7N6 | QFN68 7×7-T0.9-P0.35 | 32Mb OPI-PSRAM + 128Mb NOR Flash | 锂电池供电 | 3000 |
| SF32LB573UB766 | QFN68 7×7-T0.9-P0.35 | 32Mb OPI-PSRAM + 128Mb NOR Flash + 64Mb NOR Flash | 锂电池供电 | 3000 |
| SF32LB573UB776 | QFN68 7×7-T1.1-P0.35 | 32Mb OPI-PSRAM + 128Mb NOR Flash + 128Mb NOR Flash | 锂电池供电 | 3000 |
| SF32LB575UCNN6 | QFN68 7×7-T0.9-P0.35 | 64Mb OPI-PSRAM | 锂电池供电 | 3000 |
| SF32LB575UC7N6 | QFN68 7×7-T0.9-P0.35 | 64Mb OPI-PSRAM + 128Mb NOR Flash | 锂电池供电 | 3000 |
| SF32LB577UDNN6 | QFN68 7×7-T0.9-P0.35 | 128Mb OPI-PSRAM | 锂电池供电 | 3000 |

> 说明：电池系列**订购信息（Table 6-1）目前仅列 QFN68 料号**；但 **QFN80（57xY）/ BGA112（57xV）封装已在 DS5701 第 5 章完整给出**（管脚分布图 5-2/5-3、封装尺寸 5.3.2/5.3.3、最小系统 5.5.2/5.5.3），即本系列**支持该两种封装**——仅 Table 6-1 尚未列出对应可订货料号（规格书订购表待补全，**非芯片能力缺陷**）。命名：封装位 **U=QFN68、Y=QFN80、V=BGA112**；末位 **N6**=工业级（‑40~+85°C，本册唯一温度等级）。3.3V 系列（DS5702）Table 6-1 已含 QFN80×2 / BGA112×1。

## 关联页面
- 芯片实体（电池系列）：[[SF32LB57x_batt]]｜家族总览：[[SF32LB57]]｜3.3V 系列规格书：[[SF32LB57X_Datasheet_V0.2.2]]
- 电源外设（双系列差异）：[[SF32LB57_Peripheral_Power]]
- 其余外设：[[SF32LB57_Peripheral_GPIO]]、[[SF32LB57_Peripheral_Clock]]、[[SF32LB57_Peripheral_GPADC]]、[[SF32LB57_Peripheral_Audio]]、[[SF32LB57_Peripheral_BLE_RF]]、[[SF32LB57_Peripheral_Serial]]、[[SF32LB57_Peripheral_Memory]]、[[SF32LB57_Peripheral_Timer_PWM]]、[[SF32LB57_Peripheral_Display]]、[[SF32LB57_Peripheral_USB]]、[[SF32LB57_Peripheral_TSEN]]、[[SF32LB57_Peripheral_Security]]
- 历史版本：[[SF32LB57x_batt_Datasheet_V0.2_20260909]]

## 原始来源完整引用
1. [DS5701-SF32LB57x-芯片技术规格书-V0p2p2-20261102.pdf]，Raw：`raw/mcu/all_chip/datasheets/DS5701-SF32LB57x-芯片技术规格书-V0p2p2-20261102.pdf`，85 页。

## 待核实问题
1. **充电模块参数（Table 4-20）**：Vbus 4.5~5.5V、BAT 漏电流 30nA、恒流充电 5~400mA、涓流 0.5~30~40mA、截止电流比 10% Icc、截止电压 Vcv 4.2~4.45V、满电精度 ±1.5%、涓流转恒流阈值 3V、复充阈值 Vcv‑0.15V、过充保护 4.78V。充电模块为 die 共用内置功能；3.3V QFN68 封装未引出 VBUS/VBAT，按引脚表判定 3.3V 系列不接充电。
2. **电池系列 GPIO 数量已闭环**：DS Table 5-3 标 QFN68 = 47 GPIO（与 3.3V QFN68 相同，同封装两系列一致）；旧「电池比 3.3V 少 1」口径作废。UM Table 5-1 / pinmux 表显示 12 引脚未引出（PA21 + PA45–PA54 + PA57）推得 46，与 DS 差 1，按用户判定 **以 DS 为准 = 47**。
3. **功耗数据已在 V0.2.2 补充**（Table 4-5/4-6 由 TBD 变为实测），但**仍为草稿**，量产前须复核；电池系列处理器功耗以 3.3V 测得，BT/BLE 以 3.8V 测得，测试电压与 3.3V 系列（处理器@3.8V / BT-BLE@3.3V）交叉，比较需注意。
4. **QFN80 / BGA112 封装**：DS5701 第 5 章已完整给出其管脚分布、封装尺寸与最小系统（57xY/57xV），即电池系列**支持**这两种封装；仅 Table 6-1 订购信息暂未列出对应料号（规格书订购表待补全，非芯片能力缺陷，亦非"后续提供"）。
5. 复位 / BOOT 外部电路、PCB 布局与 EMC/ESD 规范仍缺失；仍待补硬件设计指南（HDG）/参考设计（RF 设计）。
