---
title: SF32LB57 片上温度传感器（TSEN）
type: mcu_peripheral
aliases: ["SF32LB57_Peripheral_TSEN","SF32LB57温度传感器","SF32LB57 TSEN"]
tags: ["企业MCU","外设-模拟","芯片系列-SF32LB57","原理图审图/故障分析/报告输出"]
sources: [SF32LB57X_Datasheet_V0.2.2],[SF32LB57x_batt_Datasheet_V0.2.2],[UM5701-SF32LB57x-用户手册_V0p1]]
mcu_family: ["SF32LB57"]
series_applicability: ["通用-两系列共用"]
mcu_model: ["SF32LB57X","SF32LB57x"]
hardware_version: ["QFN68 7x7","QFN80 8x8","BGA112 5.4x4.4","UM5701 V0.1"]
created: 2026-11-02
updated: 2026-11-02
---

## 摘要
SF32LB57 集成**片上温度传感器 TSEN**，通过采样内部温敏电阻的电压将温度转换为数字编码，用于系统实时监控芯片结温。**两系列（3.3V / 电池）一致**。

- 分辨率：**0.2°C**
- 测温范围：**−40°C ~ 125°C**
- 读数与式：支持**轮询（TSEN_IRSR）或中断（TSEN_IRQ）**两种方式
- 基准地址：**0x50089000**

> 📌 TSEN 与 GPADC **共用 Bandgap（带隙基准）**，UM 建议 TSEN 关闭后也不要关闭 Bandgap，以免影响 GPADC。

## 1. 主要特性（UM §8.2.2，印刷 p191）
- 分辨率 0.2°C
- 支持温度范围 −40°C 到 125°C
- 支持轮询或中断方式读数

## 2. 功能描述（UM §8.2.3，印刷 p191–192）
### 2.1 时钟
TSEN 工作时钟由 **LPSYS 的 PCLK 分频**而来，分频比由 `TSEN_CTRL_REG.ANAU_TSEN_CLK_DIV` 设置：
`f_tsen = f_pclk / ANAU_TSEN_CLK_DIV`。UM 要求**时钟频率应为 1MHz 或 2MHz**。

### 2.2 读数换算
转换结果从 `TSEN_RDATA` 读取，12-bit 数据，按以下厂商公式换算为温度：
`Temp = (Dec(TSEN_RDATA) + 3000) / 10100 × 749.2916 − 277.5391`

### 2.3 读数流程
- **轮询**：启动后轮询 `TSEN_IRQ.TSEN_IRSR`，为 1 表示转换完成；读数后将 `TSEN_IRQ.TSEN_ICR` 置 1 清零。
- **中断**：使能 TSEN 中断（`TSEN_IRQ.TSEN_IMR` 置 0），转换完成产生 `TSEN_IRQ`；处理中断时 `TSEN_ICR` 置 1 清中断并读数。

### 2.4 使用流程
1. 按 PCLK 频率配置分频比，使 TSEN 时钟为 **1MHz 或 2MHz**。
2. `HPSYS_CFG.ANAU_CR.EN_BG` 置 1，打开 Bandgap。
3. `TSEN_CTRL_REG.ANAU_TSEN_EN` 置 1，使能 TSEN 数字模块。
4. `ANAU_TSEN_PU` 置 1、`ANAU_TSEN_RUN` 置 0。
5. `ANAU_TSEN_RSTB` 先置 0 再置 1（低电平至少 20µs）。
6. `ANAU_TSEN_RUN` 置 1，轮询/中断读数，或等待约 **3ms** 绝对时间后读数。
7. 关闭：`ANAU_TSEN_RUN / PU / EN` 置 0，`ANAU_TSEN_RSTB` 置 1。
8. Bandgap 与 GPADC 共用，推荐不关闭（如需关闭勿干扰 GPADC）。

## 3. 寄存器（UM §8.2.4，印刷 p191–192，基地址 0x50089000）
| Offset | 寄存器 | 关键位 |
|---|---|---|
| 0x00 | TSEN_CTRL_REG | ANAU_TSEN_CLK_DIV[17:12]、ANAU_TSEN_EN[11]、ANAU_TSEN_RDY[10]、ANAU_TSEN_RUN[2]、ANAU_TSEN_RSTB[1]、ANAU_TSEN_PU[0] |
| 0x04 | TSEN_RDATA | TSEN_RDATA[11:0]，12-bit 温度数字码 |
| 0x08 | TSEN_IRQ | TSEN_ISR[3]、TSEN_IRSR[2]、TSEN_IMR[1]、TSEN_ICR[0] |

## 4. 硬件设计注意
- TSEN 为**纯内部片上**模块，无外部引脚与外围器件；原理图层面无专用连接要求。
- 与 GPADC 共用 Bandgap：测温与模拟采样（电池电压/外部通道）同属模拟前端，二者使能顺序与 Bandgap 状态需协调（见 [[SF32LB57_Peripheral_GPADC]]）。
- 测温用于系统热保护/结温监控，软件策略（采样周期、阈值告警）由应用决定，UM 未给具体电气限值。

## 关联页面
- 家族：[[SF32LB57]]｜3.3V：[[SF32LB57X]]｜电池：[[SF32LB57x_batt|SF32LB57x]]
- 模拟前端：[[SF32LB57_Peripheral_GPADC]]（共用 Bandgap）
- 来源：[[UM5701-SF32LB57x-用户手册_V0p1]]

## 原始来源完整引用
1. [UM5701-SF32LB57x-CN V0.1]，第 8 章 模拟外设 §8.2 温度传感器 TSEN（印刷 p190–192）：简介、主要特性、功能描述（时钟/读数换算/读数流程/使用流程）、寄存器映射表 8-2（基地址 0x50089000）。

## 待核实问题
1. TSEN 测温精度（非分辨率）与校准参数 UM 未给出典型误差，实际量产偏差待官方数据/实测。
2. TSEN 与 GPADC 共用 Bandgap 的具体上电/采样时序约束，UM 仅建议「不关闭 Bandgap」，深度低功耗（Hibernate）下 TSEN 是否可工作待确认。
