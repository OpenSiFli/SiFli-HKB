---
title: SF32LB57 GPIO 与 IO 特性
type: mcu_peripheral
aliases: ["SF32LB57_Peripheral_GPIO","SF32LB57 GPIO","SF32LB57 IO特性"]
tags: ["企业MCU","外设-GPIO","芯片系列-SF32LB57","原理图审图/故障分析/报告输出"]
sources: [[SF32LB57X_Datasheet_V0.2_20260909],[SF32LB57x_batt_Datasheet_V0.2_20260909],[SF32LB57X_Datasheet_V0.2.2],[SF32LB57x_batt_Datasheet_V0.2.2],[UM5701-SF32LB57x-用户手册_V0p1]]
mcu_family: ["SF32LB57"]
series_applicability: ["通用-两系列共用"]
mcu_model: ["SF32LB57X","SF32LB57x"]
hardware_version: ["QFN68 7x7","QFN80 8x8","BGA112 5.4x4.4","Datasheet V0.2"]
created: 2026-09-09
updated: 2026-09-09
---

## 摘要
SF32LB57 家族 GPIO 与 IO 电气特性。**两系列 IO 电气特性完全相同**，差异仅在：**QFN68 版本**电池系列无独立 VDDIOA 外部引脚（IO 域由内部供给），而 **QFN80/BGA112 两系列均有 VDDIOA**；PA 引出由封装决定（QFN68 = 47、QFN80/BGA112 = 58，两系列同封装相同），与供电系列无关。

## 1. GPIO 资源
- 大核域 GPIO：**PA00–PA57，最多 58 个**（Pinmux 见 [[SF32LB57_Pinmux]]；完整复用矩阵见规格书 Table 5-2，P52/54（3.3V）、P53/55（电池）；UM Table 5-1 印刷 p57）
- 总数：**58 GPIO + 6 GPO MPI IO**（PA00–PA57，家族特性）
- 按封装（**双系列一致**，PA 引出由封装定）：**QFN68 = 47 GPIO**（以 DS Table 5-3 为准；UM Table 5-1 / pinmux 显示 12 引脚未引出，与 DS 的 47 偏差，按 DS 为准）；**QFN80 = 58 GPIO**；**BGA112 = 58 GPIO + 6 GPO MPI IO**
- 电池系列订购信息（Table 6-1）当前仅列 QFN68 料号，但 **QFN80(57xY)/BGA112(57xV) 封装 DS5701 第 5 章已完整给出（管脚/尺寸/最小系统），系列支持该两种封装**，仅 Table 6-1 暂未列其料号（规格书订购表待补全，非芯片能力缺陷）
- **PA24–PA27 支持低功耗 IO**（低功耗模式下仍可输出，UM §5.8）
- **FreeIO GPIO**：支持灵活配置

## 2. IO 电气特性（Table 4-3 @3.3V，两系列一致）
| 参数 | 符号 | 条件 | Min | Typ | Max | 单位 |
|---|---|---|---|---|---|---|
| 管脚电容 | CIN | | 2.5 | 3 | 3.5 | pF |
| 高电平输入电压 | VIH | | 0.7×VDD | | VDD | V |
| 低电平输入电压 | VIL | | VSS | | 0.3×VDD | V |
| 高电平输入电流 | IIH | | | 10 | 40 | nA |
| 低电平输入电流 | IIL | | | 10 | 40 | nA |
| 高电平输出电压 | VOH | 高阻负载 | 0.8×VDD | | VDD | V |
| 低电平输出电压 | VOL | 高阻负载 | VSS | | 0.2×VDD | V |
| 高电平驱动电流 | IOH | VOH=0.8×VDD，最大驱动强度 | 12 | 30 | 38 | mA |
| 低电平驱动电流 | IOL | VOH=0.2×VDD，最大驱动强度 | 12 | 30 | 38 | mA |
| 内部上拉电阻 | RPU | Vpad=0.8×VDD | 7 | 10 | 20 | kΩ |
| 内部下拉电阻 | RPD | Vpad=0.2×VDD | 7 | 10 | 20 | kΩ |
| 复位释放电压 | VIH_nRST | | 0.7×VDD | | VDD | V |
| 复位电压 | VIL_nRST | | VSS | | 0.3×VDD | V |

**驱动能力档位（Table 4-21）**
| DS1 | DS0 | 驱动能力 |
|---|---|---|
| 0 | 0 | 2 mA |
| 0 | 1 | 4 mA |
| 1 | 0 | 8 mA |
| 1 | 1 | 12 mA |

> 注：Table 4-3 的 12/30/38mA 为**最大驱动强度**下的实测（VOH=0.8×VDD 测试条件）；Table 4-21 为可配置四档。二者为同一驱动电路在不同配置/测试条件下的结果（与 SF32LB52 家族结论一致）。

## 3. 双系列 GPIO 差异（QFN68 同封装）
| 项目 | 3.3V 系列 SF32LB57X | 电池系列 SF32LB57x |
|---|---|---|
| GPIO 电源域 | **QFN68 由内部供给**；**QFN80/BGA112 由 VDDIOA(21) 外部独立供电** | **QFN68 由内部供给**；**QFN80/BGA112 由 VDDIOA 外部供电（1.8/2.5/3.3V，UM Table 3-3）** |
| QFN68 PA GPIO 数 | **47**（以 DS Table 5-3 为准；UM Table 5-1 / pinmux 显示 12 引脚未引出，与 DS 偏差，按 DS 为准） | **47**（以 DS 为准；PA 引出由封装定，非供电系列定） |
| 其它电源专用引脚差异 | PVDD / VBATS / VDDIOA / NC | VSYS / VBAT / VBUS / VDD33_VOUT1 / VCC（详见 [[SF32LB57_Peripheral_Power]]） |

## 4. 硬件设计约束
- 使能输入的数字 IO **不得悬空**，须配置确定电平（避免低功耗漏电升高）。
- IO 电平域须与对端器件匹配；3.3V 系列由 VDDIOA 决定，电池系列由内部电源决定。
- 驱动强度按负载选择（默认/2/4/8/12mA），大电流负载注意总功耗与压降。
- 内部上下拉典型 10kΩ（7~20kΩ），精密/低功耗场合优先外部电阻。
- **pin22（3.3V QFN68）为 NC，必须悬空**，不可作 GPIO。
- **PA34 电源键（PWRKEY）**：默认带 PWRKEY 硬件功能（长按高电平触发复位）；需当普通 GPIO 时，将 **PMUC WER.PWRKEY 置 0** 关闭该功能后无高电平时长限制（见 [[DR_PA34_PowerKey_GPIO]]）。

## 5. UM5701 V0.1 补充（寄存器级手册）
- **PA21 在 QFN68 未引出**（UM Table 5-1）：故 QFN68（两系列）做 Hibernate 合封存储供电控制须用 **PA11（MPI2_VDD_EN/SD_VDD_EN，默认 PU）** 替代；QFN80/BGA112 版本 PA21 可用（详见 [[CR_Hibernate_SiP_Supply]]）。修正旧「PA21 仅 3.3V 有」口径（原指 52 家族）。
- **PA24–PA27 支持低功耗 IO**（低功耗模式下仍可输出，UM §5.8）。
- **IO 上下拉**：PE/PS 控制，PE=1/PS=0 下拉、PE=1/PS=1 上拉，阻值约 10k~40kΩ（UM §5.4）；**PA43/PA44 默认 SWD**（FSEL=1），其余默认 GPIO（FSEL=0）；**PA18/PA19 在 BOOTMODE1 被 BOOTROM 配为 USART1_RXD/TXD**。
- **避免 IO 漏电**：使能输入的数字 IO 不得悬空（UM §5.12）；合封 IO（SA/SB）防漏电见 [[CR_Hibernate_SiP_Supply]]。


## 关联页面
- 家族：[[SF32LB57]]｜3.3V：[[SF32LB57X]]｜电池：[[SF32LB57x_batt|SF32LB57x]]
- 电源：[[SF32LB57_Peripheral_Power]]｜复用矩阵：[[SF32LB57_Pinmux]]
- PA34 电源键作 GPIO：[[DR_PA34_PowerKey_GPIO]]
- 来源：[[SF32LB57X_Datasheet_V0.2_20260909]]、[[SF32LB57x_batt_Datasheet_V0.2_20260909]]、[[UM5701-SF32LB57x-用户手册_V0p1]]

## 原始来源完整引用
1. [DS5702-S32LB57X V0.2]，Table 4-3（P39）、Table 4-21（P48）、Table 5-2（P52/54）、Table 5-3（P61）。
2. [DS5701-SF32LB57x V0.2]，Table 4-3（P40）、Table 4-21（P49）、Table 5-2（P53/55）、Table 5-3（P62）。
3. [UM5701-SF32LB57x-CN V0.1]，第 5 章 GPIO（印刷 p57–96，PDF p72–111）：Table 5-1 PA00–PA57 管脚列表、IO 结构、上下拉、低功耗 IO、漏电。

## 待核实问题
1. 完整 PA 复用矩阵（Table 5-2）为多页表格，本次未逐行转录，详见 [[SF32LB57_Pinmux]]（导入后）与规格书原表。
2. **QFN68 GPIO 数已按 DS 闭环**：DS Table 5-3 标 47；UM Table 5-1 与 pinmux 显示 12 引脚未引出（PA21 + PA45–PA54 + PA57），与 DS 的 47 偏差，按用户判定 **以 DS 为准 = 47**。
3. 默认上电状态（PU/PD/高阻）逐引脚表未在规格书文本中完整给出，待补用户手册/参考设计。
