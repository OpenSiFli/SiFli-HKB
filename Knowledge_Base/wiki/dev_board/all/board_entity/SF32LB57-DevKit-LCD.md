---
title: SF32LB57-DevKit-LCD
type: dev_board
aliases: ["57 LCD 开发板", "SF32LB57 DevKit LCD", "LCD 开发板"]
tags: ["开发板", "板卡系列-SF32LB57", "型号-SF32LB57-DevKit-LCD", "板卡实体", "LCD开发板", "原理图审图/故障分析/报告输出"]
sources: [[SF32LB57-DevKit-LCD_UserGuide]]
board_model: ["SF32LB57-DevKit-LCD"]
hardware_version: ["V1.1（原理图 SCH_V1_1，板名 SF32LB57-DevKit-LCD-V1_1）", "V1.0（早期版；V1.1 变更：CH343P 增加 DTR/RTS# 控制、RGB-LED 由 PA11 改为 PA08）"]
platform_family: ["SF32LB57"]
chip_model: ["SF32LB57X"]
module_model: ["SF32LB57-MOD-WD（N16R8 / R16N16 两变体，共用兼容 PCB、靠焊接 BOM 区分）"]
mounting_type: ["模组"]
storage_config: ["模组 N16R8：外置 16MB QSPI-NOR Flash（U2 PY25Q128HA，WSON-8，MPI3 接口）+ MCU 内置 8MB PSRAM", "模组 R16N16：MCU 内置 16MB NOR + 16MB PSRAM（无外置 U2）", "两变体共用兼容 PCB，靠焊接 BOM 区分（详见 [[SF32LB57-MOD-WD_SCH_2026-08-26]]）", "板载 TF 卡座 J6（1bit SDIO）"]
power_supply: ["USB Type-C 5V 供电", "主 3.3V：ETA3425D2G Buck(U4) → VDD_3V3", "Wi-Fi 3.3V：ETA3425D2G Buck(U7) → WF_3V3（由 IOEX_WF_PWREN 控制）"]
peripherals: ["DPI-RGB888 显示(FPC1 40P 0.5mm)", "DCMI DVP 摄像头(FPC4 24P 0.5mm)", "音频 NS4150B Class-D + 双 PDM 麦克风", "USB2.0 FS(PA35/PA36)", "USB-to-UART(CH343P)", "TF 卡 SDIO", "Wi-Fi/BT Combo(AIC8800D40LS-N, SDIO)", "40P 树莓派兼容排针 J3", "3 路 GPADC 按键(PA34)", "3 LED(AW9523B) + WS2812B(PA11)", "IO 扩展器 AW9523BTOR(U11)"]
debug_interface: ["USB-to-UART(CH343P，推荐下载/调试)", "SWD(SWDIO PA44 / SWCLK PA43)"]
use_cases: ["智能 HMI", "可视对讲", "智能穿戴", "多媒体 AIoT 评估与研发调试"]
guide_doc: [[SF32LB57-DevKit-LCD_UserGuide]]
drawings: ["SF32LB57-DevKit-LCD 底板原理图 V1.1：[[SF32LB57-DevKit-LCD_SCH_V1_1]]", "SF32LB57-MOD-WD 模组原理图(2026-08-26)+双BOM(N16R8/R16N16)：[[SF32LB57-MOD-WD_SCH_2026-08-26]]", "IO 配置表：[[raw/dev_board/gpio_table/57x LCD-DevKit ioconfig-11022026.xlsx]]", "模组 BOM：[[raw/dev_board/bom/BOM_SF32LB57-MOD-WD-N16R8_2026-08-26.xlsx]]、[[raw/dev_board/bom/BOM_SF32LB57-MOD-WD-R16N16_2026-10-21.xlsx]]", "PCB：未提供（待补）"]
created: 2026-11-03
updated: 2026-11-03
---

## 摘要
SF32LB57-DevKit-LCD 是基于 **SF32LB57-MOD-WD 模组**（搭载 **SF32LB57X** 3.3V 供电系列双核 Cortex-M33 @240MHz）的 **LCD 多媒体 AIoT 评估/开发板**，面向 DPI-RGB 并行显示、DCMI 摄像头、音频、Wi-Fi/BT 等场景的研发调试与演示。属**开发板演示方案**，量产以企业硬件设计指南 / 芯片 datasheet 为准。

## 一、平台（使用的 MCU 系列）
- 平台系列：[[SF32LB57]]（SF32LB57 全系列，含 3.3V 与电池供电两分支）
- 说明：本开发板基于 SF32LB57 系列设计，可复用其外设规范与校验规则；本板具体搭载 3.3V 供电分支（见芯片节）。

## 二、芯片（具体型号）
- 芯片型号：[[SF32LB57X]]（3.3V 供电系列，双核 Arm Cortex-M33 STAR-MC1 @240MHz，1968 CoreMark；片上 SRAM 592KB：HPSYS 512KB + LPSYS 80KB；合封 QSPI-NOR Flash 与可选 OPI-PSRAM）
- 关键参数以双链芯片实体页为准，本页不重复展开。

## 三、模组（具体型号）
- 模组型号：**SF32LB57-MOD-WD**（邮票孔/焊盘封装，便于贴片或焊接于底板）
- 模组内部集成：SF32LB57X 芯片、合封 QSPI-NOR Flash（启动 Flash）+ 可选 OPI-PSRAM、AIC8800D40LS-N 系列 Wi-Fi 芯片（SDIO 接口）、48MHz 晶体（SF32LB57X）+ 40MHz 晶体（AIC8800D40LS-N）
- 模组引出管脚分配见使用指南「SF32LB57-MOD-WD 模组 GPIO 分配」表。

## 四、上芯方式
- 方式：**模组**（SF32LB57-MOD-WD 模组贴装于底板）
- 说明：板载为模组而非裸芯片，最小系统、电源、调试接口形态由模组管脚决定。

## 五、存储配置
- 板载 Flash / RAM：模组合封 QSPI-NOR Flash（启动 Flash，依具体模组料号）；可选 OPI-PSRAM（依模组料号）
- 扩展存储接口：TF 卡座 J6（Micro SD，1bit SDIO）；相关规范见 [[SF32LB57_Peripheral_Memory]]

## 六、供电方式
- 输入电压：USB Type-C **5V**（板上两个 Type-C 均接入 5V VBUS，可供电；USB-to-UART 口用于下载/调试/供电，USB2.0 FS 口用于从设备功能/供电）
- 板载电源树：
  - 主 3.3V：ETA3425D2G Buck（U4）→ **VDD_3V3**（主板主电源、MCU、LCD、外设）
  - Wi-Fi 3.3V：ETA3425D2G Buck（U7）→ **WF_3V3**（模组内部 Wi-Fi/BT Combo）；U7 使能由 IO 扩展器信号 **IOEX_WF_PWREN** 控制，软件可独立关闭 Wi-Fi 电源以降低功耗
  - IO 扩展器：AW9523BTOR（U11），I2C（PA_22 SCL / PA_23 SDA），扩展 16 路可控 IO
- 上电/掉电时序要点：电源树示意见使用指南「供电说明」电源树表；Wi-Fi 电源受 IOEX_WF_PWREN 软控。

## 七、外设
> 以下板载外设与接口均为**开发板演示电路/连接**，量产以企业硬件设计指南为准；具体接线、上下拉、复用关系见使用指南与芯片规格书 Pinmux。

- **显示**：DPI-RGB 并行显示，FPC1（40P 0.5mm）默认 RGB888（软件可配 RGB565/666），ePicasso 3.0 硬件加速最高 2048×2048@60fps，背光 BL_PWM(PA_11)；规范见 [[SF32LB57_Peripheral_Display]]
- **摄像头**：DCMI DVP 摄像头，FPC4（24P 0.5mm），支持 GC032A/OV2640/OV5640/OV3660；板载独立 LDO（DVDD_1V5 / DVDD_2V8 / AVDD_2V8）
- **音频**：24-bit DAC → NS4150B Class-D 功放（U8）驱动扬声器（J7 MX1.25-2P，默认 8Ω）；双 PDM 麦克风（MIC1/MIC2，PA_56 PDM_CLK / PA_55 PDM_DAT，由 IOEX_PDM_PWREN 控制）；规范见 [[SF32LB57_Peripheral_Audio]]
- **USB**：USB2.0 FS（PA35/PA36，直连芯片）；USB-to-UART（CH343P，下载/调试）；规范见 [[SF32LB57_Peripheral_USB]]
- **无线**：模组内 AIC8800D40LS-N Wi-Fi/BT Combo（SDIO），IPEX-1 天线座；规范见 [[SF32LB57_Peripheral_BLE_RF]]
- **通用 IO / 扩展**：40P 树莓派兼容排针 J3（引出 GPIO/I2C/SPI/UART/SWD）；3 路 GPADC 按键（KEY-1/2/3，共用 PA_34）；3 用户 LED（红/绿/蓝，AW9523B P1_1~P1_3）+ WS2812B（PA_11）；规范见 [[SF32LB57_Peripheral_GPIO]]、[[SF32LB57_Peripheral_GPADC]]、[[SF32LB57_Peripheral_Timer_PWM]]
- **调试与下载**：见 §八

## 八、调试接口
- **USB-to-UART 下载（推荐）**：CH343P Type-C 口；BOOT MODE 跳线帽（J1）进入下载模式；SifliTrace / Impeller 下载
- **SWD 调试**：SWDIO = PA_44，SWCLK = PA_43；可接 J-Link / DAP-Link；调试器 VCC 接 3.3V、GND 共地
- 相关规范见 [[SF32LB57_Peripheral_GPIO]]（SWD 复用）与 MCU 侧调试设计规则（待补）

## 九、适用场景
- 智能 HMI、可视对讲、智能穿戴等产品的研发评估；DPI-RGB 显示、DCMI 摄像头、音频、Wi-Fi/BT 多媒体接口的驱动开发与演示。

## 十、使用指南文档
- 指南文档：[[SF32LB57-DevKit-LCD_UserGuide]]（原始来源 `raw/dev_board/guide/SF32LB57-DevKit-LCD.md`，SiFli Wiki 使用指南，忠实导入，源文件未修改）
- 定位：实物配套实操文档，优先级低于企业硬件设计指南、芯片 datasheet/用户手册。

## 十一、图纸（原理图 / PCB / BOM / GPIO 分配表）
- 底板原理图：**SF32LB57-DevKit-LCD_SCH_V1_1**（V1.1，13 页）→ [[SF32LB57-DevKit-LCD_SCH_V1_1]]（电源树/复位/时钟/BOOT/调试/存储/显示/摄像头/音频/无线/IO 扩展器/LED/40P 排针 + 整板 GPIO 分配）
- 模组原理图 + BOM：**SF32LB57-MOD-WD_SCH_2026-08-26**（3 页）+ N16R8/R16N16 双 BOM → [[SF32LB57-MOD-WD_SCH_2026-08-26]]（共用兼容 PCB、靠焊接区分两型号）
- IO 配置表：[[raw/dev_board/gpio_table/57x LCD-DevKit ioconfig-11022026.xlsx]]（58 MCU GPIO + 16 IO 扩展器 + CASE 配置）
- PCB：未提供（待补）

## 关联页面
- 来源摘要：[[SF32LB57-DevKit-LCD_UserGuide]]
- 图纸提取页：[[SF32LB57-DevKit-LCD_SCH_V1_1]]（底板 V1.1）、[[SF32LB57-MOD-WD_SCH_2026-08-26]]（模组+双BOM）
- 平台/芯片：[[SF32LB57X]]、[[SF32LB57]]
- 外设规范：[[SF32LB57_Peripheral_GPIO]]、[[SF32LB57_Peripheral_Display]]、[[SF32LB57_Peripheral_USB]]、[[SF32LB57_Peripheral_Audio]]、[[SF32LB57_Peripheral_Memory]]、[[SF32LB57_Peripheral_BLE_RF]]、[[SF32LB57_Peripheral_Power]]
- 硬件设计指南（量产以该文档为准）：[[SF32LB57_Hardware_Design_Guide]]
- 校验规则（审图复用）：暂无，待补原理图后生成

## 原始来源完整引用
1. [SF32LB57-DevKit-LCD 开发板使用指南]，Raw 存储路径：`raw/dev_board/guide/SF32LB57-DevKit-LCD.md`（SiFli Wiki 源 `D:/Github/SiFli-Wiki/source/zh_CN/board/sf32lb57x/SF32LB57-DevKit-LCD.md`），文档类型：使用指南，硬件版本：V1.0/V1.1（待核实）。

## 待核实问题
1. ~~**图纸缺失**~~ → 已补充：底板原理图 V1.1、MOD-WD 模组原理图 + N16R8/R16N16 双 BOM、IO 配置表均已导入并建立图纸提取页（[[SF32LB57-DevKit-LCD_SCH_V1_1]]、[[SF32LB57-MOD-WD_SCH_2026-08-26]]）；「图纸」项已闭环。PCB 仍待补。
2. **硬件版本**：指南正文仅在 WS2812B 连接处提及 V1.0（PA_11）与 V1.1（PA_08）差异，未给出板级单一版本标识，待确认。
3. ~~**模组料号与存储**~~ → 已明确：模组 SF32LB57-MOD-WD 两变体 MCU 料号 N16R8=`SF32LB57EYBBN6`、R16N16=`SF32LB57GYD7N6`（均 QFN80）；N16R8 外置 16MB NOR(U2 PY25Q128HA)+内置8MB PSRAM，R16N16 内置16MB NOR+16MB PSRAM（U2 DNP）；共用兼容 PCB、靠焊接区分（见 [[SF32LB57-MOD-WD_SCH_2026-08-26]]）。
4. **RGB888 封装依赖**：RGB888 全位宽需 QFN80/BGA112 封装模组；本板使用具备完整 RGB888 及 DVP 接口的模组料号（指南声明），具体模组封装待核实。
