---
title: CR-SF32LB57 VDD_VOUT1电容规格
type: mcu_check_rule
aliases: ["CR_SF32LB57_VDD_VOUT1_Capacitor","SF32LB57 VDD_VOUT1电容审图规则"]
tags: ["企业MCU","AI审图规则","原理图审图","风险等级-严重"]
sources: [[SF32LB57_Hardware_Design_Guide]]
mcu_family: ["SF32LB57"]
series_applicability: ["通用-两系列共用"]
mcu_model: ["SF32LB57X","SF32LB57x"]
hardware_version: ["QFN68 7x7","QFN80 8x8","BGA112 5.4x4.4"]
risk_level: 严重
created: 2026-11-19
updated: 2026-11-19
---

## 摘要
校验 SF32LB57 家族（3.3V 系列 SF32LB57X 与电池系列 SF32LB57x 均适用）**VDD_VOUT1 引脚外接电容是否为 10µF/6.3V 且靠近引脚**。**注意：SF32LB52 家族该脚要求为 4.7µF/6.3V，两家族要求不同，禁止跨家族套用。**

## 核心硬件内容（背景）
- **VDD_VOUT1** 为内部 LDO1 输出（HPSYS 核心电源，约 1.1V），仅需外接滤波电容到地，属内部电源，**不给外设供电、禁止外部供电**；电容就近摆放可降低核心电源纹波、提升系统稳定性。
- **SF32LB57 要求（本规则）**：VDD_VOUT1 外接 **10µF/6.3V** 电容，靠近引脚摆放。依据 [[SF32LB57_Hardware_Design_Guide]] 设计自查表第 31 项；该指南修订历史 **0.0.2（11/2026）** 已将外接电容由 4.7µF/6.3V 改为 10µF/6.3V，最小系统原理图同步升级至 V3（6 张，UH/UL/YH/YL/VH/VL）。
- **SF32LB52 要求（对照，不适用本规则）**：[[CR_Checklist_Power]] 第 31 项为 **4.7µF/6.3V**，来源是 SF32LB52 官方 Checklist，`mcu_family` 仅 SF32LB52。审 57 板卡时**不得套用此值**。

## 专项业务内容 —— 原理图 AI 审图规则（必填）
1. **判定条件**：原理图解析到 SF32LB57 家族芯片（型号含 `SF32LB57`，含 SF32LB57X / SF32LB57x 两系列）的 **VDD_VOUT1 网络对地滤波电容**时触发。核对容值、耐压与摆放位置。
2. **违规现象描述**：
   - VDD_VOUT1 对地电容**容值不是 10µF**（典型误用：**沿用 SF32LB52 的 4.7µF**，或容值过小/缺失）；
   - 电容**耐压低于 6.3V**；
   - 电容**未靠近 VDD_VOUT1 引脚**摆放（走线过长，去耦失效）；
   - VDD_VOUT1 被**外部供电**或被外设**取电**（该脚为内部 LDO 输出，禁止外供/带载）。
3. **风险影响说明**：去耦不足或耐压不足 → HPSYS 核心电源纹波过大、电容失效 → 芯片**复位/死机/工作不稳定**；外部供电则可能与内部 LDO 对冲造成**器件损坏**（后者按 [[CR_SF32LB57_PowerSeries]] 致命级处理）。
4. **标准化整改方案**：
   - VDD_VOUT1 引脚就近放置 **10µF/6.3V**（或更高耐压）陶瓷电容到地；
   - **不要套用 SF32LB52 的 4.7µF 要求**（两家族电容规格不同）；
   - 确认 VDD_VOUT1 仅接电容到地，不接外部电源、不带外设负载；参考 [[SF32LB57_Hardware_Design_Guide]] §最小系统原理图 **V3** 版。
5. **知识库溯源依据**：[[SF32LB57_Hardware_Design_Guide]] 设计自查表第 31 项 + 修订历史 0.0.2；最小系统原理图 V3（`raw/mcu/all_chip/hw_design/images/57x/SF32LB57-{UH,UL,YH,YL,VH,VL}-MHS-SCH-V3.svg`）；[[SF32LB57_Peripheral_Power]]；家族差异对照见 [[CR_Checklist_Power]] 第 31 项（52 家族 4.7µF）。

## 关联页面
- 家族：[[SF32LB57]]｜3.3V：[[SF32LB57X]]｜电池：[[SF32LB57x_batt|SF32LB57x]]
- 供电审图：[[CR_SF32LB57_PowerSeries]]、[[CR_SF32LB57_AbsMaxRatings]]
- 家族差异对照（52 家族 4.7µF）：[[CR_Checklist_Power]] 第 31 项
- 来源指南：[[SF32LB57_Hardware_Design_Guide]]｜电源外设：[[SF32LB57_Peripheral_Power]]
- 来源：[[SF32LB57X_Datasheet_V0.2.2]]、[[SF32LB57x_batt_Datasheet_V0.2.2]]

## 原始来源完整引用
1. [SF32LB57x 硬件设计指南（SiFli Wiki）]：`D:/Github/SiFli-Wiki/source/zh_CN/hardware/SF32LB57x-HW-Application.md`（源文件只读），§设计自查表第 31 项、§修订历史 0.0.2；转录页 [[SF32LB57_Hardware_Design_Guide]]。
2. 最小系统原理图 V3（6 张），原件位于源仓库 `source/zh_CN/hardware/assets/57x/`，副本 `raw/mcu/all_chip/hw_design/images/57x/`。

## 待核实问题
- 无（10µF/6.3V 由官方指南 v0.0.2 明确；与 SF32LB52 官方 Checklist 的 4.7µF 差异已确认，属家族差异化要求，非矛盾）。
