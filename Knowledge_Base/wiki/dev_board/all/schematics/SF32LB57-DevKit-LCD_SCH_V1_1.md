---
title: SF32LB57-DevKit-LCD 原理图提取（V1.1）
type: dev_board_sch
aliases: ["57 LCD 开发板原理图", "SF32LB57-DevKit-LCD 图纸"]
tags: ["开发板","原理图","PCB","BOM","GPIO分配表","图纸提取","SF32LB57-DevKit-LCD"]
sources: [[raw/dev_board/schematics/SF32LB57-DevKit-LCD_SCH_V1_1_2026-11-03.pdf],[raw/dev_board/schematics/SF32LB57-MOD-WD_SCH_2026-08-26.pdf],[raw/dev_board/bom/BOM_SF32LB57-MOD-WD-N16R8_2026-08-26.xlsx],[raw/dev_board/bom/BOM_SF32LB57-MOD-WD-R16N16_2026-10-21.xlsx],[raw/dev_board/gpio_table/57x LCD-DevKit ioconfig-11022026.xlsx]]
board_model: ["SF32LB57-DevKit-LCD"]
hardware_version: ["V1.1（原理图 SCH_V1_1，板名 SF32LB57-DevKit-LCD-V1_1）"]
created: 2026-11-03
updated: 2026-11-03
---

## 摘要
本页为 **SF32LB57-DevKit-LCD** 开发板（底板 + SF32LB57-MOD-WD 模组）的原理图结构化提取，是填写板卡实体 11 项参数的事实依据。原理图共 13 页（P1 框图 / P2 变更记录 / P3–P13 电路），版本 **V1.1**，PDF 提取经 EDA 矢量文本/坐标核对。模组（MOD-WD）另见 [[SF32LB57-MOD-WD_SCH_2026-08-26]]。所有数据标注原理图页码/位号/网络，禁止编造。

> 说明：本开发板为**演示方案**，量产以 [[SF32LB57_Hardware_Design_Guide]] 与芯片 datasheet 为准；板载电路仅作参考。

## 一、图纸清单
- 开发板原理图：[[raw/dev_board/schematics/SF32LB57-DevKit-LCD_SCH_V1_1_2026-11-03.pdf]]（13 页，V1.1，2026-11-03 归档）
- 模组原理图：[[raw/dev_board/schematics/SF32LB57-MOD-WD_SCH_2026-08-26.pdf]]（3 页，2026-08-26）
- 模组 BOM（N16R8）：[[raw/dev_board/bom/BOM_SF32LB57-MOD-WD-N16R8_2026-08-26.xlsx]]
- 模组 BOM（R16N16）：[[raw/dev_board/bom/BOM_SF32LB57-MOD-WD-R16N16_2026-10-21.xlsx]]
- 整板 IO 配置表：[[raw/dev_board/gpio_table/57x LCD-DevKit ioconfig-11022026.xlsx]]（58 个 MCU GPIO + 16 个 IO 扩展器脚 + CASE 配置汇总）

## 二、关键电路提取（依据原理图 EDA 矢量文本/坐标）

### 2.1 电源树（BUCK/LDO/各电源域）
- **输入**：USB Type-C 双口（USB1 设备口、USB2 调试口）均接 **5V VBUS**（USB_VBUS），可互为供电。
- **主 3.3V**：**ETA3425D2G（U4）** 同步 Buck → **VDD_3V3**（主板主电源，供 MCU/模组/LCD/外设/IO 扩展器）。Vout = 1.11×[1+R1/R2]，L2=2.2µH，C16/C17=10µF，反馈 R53=1.1kΩ / R55=2.8kΩ（分压比对应 ~3.3V）。
- **Wi-Fi 3.3V**：**ETA3425D2G（U7）** 同步 Buck → **WF_3V3**（模组内部 Wi-Fi/BT Combo 供电）；U7 的 EN 由 IO 扩展器信号 **IOEX_WF_PWREN** 控制，软件可独立关断 Wi-Fi 电源以降功耗。L3=2.2µH，C46/C47=10µF，R79/R28=2MΩ（反馈）。
- **摄像头电源**（独立 LDO）：**U9 ETA5055V280DS2F → DVDD_2V8**（2.8V）；**U10 ME6211C15M5G-N → DVDD_1V5**（1.5V）；用于 DVP 摄像头供电（各摄像头型号电压需求见 FPC4 表）。
- **PDM 麦克风电源**：**U12 ETA5055V330DS2F → PDM_3V3**（3.3V）。
- **电池/充电检测分压**：R53(1.1kΩ)/R55(2.8kΩ) 与 R79/R28(2MΩ) 等构成电压检测网络，标注阈值 **2.97V/1.62V、2.58V/1.41V、2.19V/1.19V**（与 SF32LB57 内置充电管理相关，具体映射待核实）。
- **音频功放供电**：NS4150B 由 5V 直供（经磁珠 L4/L5 PZ1005U121-1R0TF 滤波）；**注意：音频 PA + LCD 面板合计峰值电流可能超过 USB 2.0 的 500mA VBUS 上限，建议用 PD 充电器或外部供电**（原理图原注）。

### 2.2 复位与按键
- **RESET KEY**（KEY-RSTN）：系统复位按键，网络 KEY_RSTN（原理图 P13）。
- **3×GPADC 按键**（GPADCKEY）：KEY-1 / KEY-2 / KEY-3，共用 **PA_34** 区域 GPADC（PA28–PA38 为 GPADC_CH0–CH10，PA34 兼 PWRKEY）。
- **PA_34** = 电源键/长按复位（模块内 LONGPRESS_RST/WKUP1/PWRKEY）。

### 2.3 时钟/晶振
- 开发板底板**不另设**主晶振——48MHz（MCU RF，Y2 JWT）与 40MHz（AIC8800，X1 TKD）均位于 **MOD-WD 模组内部**（见模组页）。
- 板载**无独立 32.768kHz 晶体**；RTC 时钟由 PA22/PA23 复用 XTAL32K_XI/XO 提供（与 I2C 复用，见 IO 配置表）。

### 2.4 BOOT 配置（strapping）
- **J1 BOOT MODE** 跳线帽：控制 PA00/PA01 Boot 寄存器，决定启动分支（原理图 P11 表）：
  - PA00=0, PA01=0（J1 Close）→ 正常启动，PA19 不输出串口信息；PA18/19 由 DBUART 切 SWD，PA43/44 的 SWD 被禁用。
  - PA00=0, PA01 Open（J1 Open）→ 正常启动，PA19 输出启动信息；PA18/19 作 DBUART、PA43/44 作 SWD。
  - 任意一项=1 → PA19 输出信息，支持 DBUART/SWD/USB 编程；PA18/19 作 DBUART、PA43/44 作 SWD。
- 说明：PA18/19（DBUART）与 PA43/44（SWD）存在功能切换关系，由 BOOT 模式决定。

### 2.5 下载/调试接口
- **USB-to-UART（推荐下载）**：**CH343P（U3）** 接 USB2 Type-C（TYPE-C16PIN），UD+/UD- → MCU PA18/PA19（DBUART）；VIO=3.3V、VDD=5V。
  - **V1.1 变更**：新增 DTR/RTS# 控制——增加 D9、D10（BAT54WS）、Q1（AO3401A P 沟道 MOSFET）、R92（2.2kΩ）、R93（10kΩ），由 CH343P_DTR / CH343P_RTS# 控制（用于自动复位/进入下载）。
- **SWD 调试**：SWDIO=PA_44，SWCLK=PA_43；可接 J-Link / DAP-Link（PA43/44 在 BOOT=任意1 或 J1 Open 时使能 SWD）。
- **USB2.0 FS 设备口**：USB1 Type-C，USB_D+/USB_D- → MCU PA_35/PA_36。

### 2.6 外置存储
- **模组内部**：QSPI-NOR Flash（MPI3 接口，PA_12–PA_17），N16R8 焊接外置 U2（见模组页）；TF 卡座 **J6**（1-bit SDIO，PA_09/PA_10/PA_08 = SD1_CMD/SD1_D0/SD1_CLK），IOEX_TF_DET 检卡。

### 2.7 显示（DPI-RGB888）
- **FPC1**（40P 0.5mm，FPC-05FB-40PH20）：RGB888 并行显示，信号源于 MCU **PA24–PA54**（R0–R7/G0–G7/B0–B7 + CLK/HSYNC/DE/VSYNC），背光 **BL_PWM = PA_11**；触摸 I2C（PA22/PA23），**IOEX_LCD_RST** 控制 LCD 复位。
- 屏控制 SPI（LCDC1_SPI）经模组 PA00–PA07 及 PA28–PA54 区域的复用脚（软排线转 QSPI 路由，见 IO 配置表 Q/R/S 列）。
- ePicasso 3.0 硬件加速，最高 2048×2048@60fps（详见使用指南）；软件可配 RGB565/666。

### 2.8 摄像头（DCMI DVP）
- **FPC4**（24P 0.5mm，FPC-05F-24PH20）：支持 **GC032A / OV2640 / OV3660 / OV5640**；DVP 数据经 MCU PA_00/PA_06/PA_07/PA_24/PA_25/PA_26/PA_32/PA_33/PA_43/PA_44/PA_45，VSYNC/HSYNC/PCLK/MCLK 复用对应 GPIO。
- 摄像头电源：U9（2.8V）+ U10（1.5V），各型号电压不同（GC032A 2.8V、OV2640 2.8V/1.2V、OV3660 2.8V/1.5V、OV5640 2.8V/1.5V）。
- **DCMI MUX 跳线**：DVP 通过跳线选择**占用 Wi-Fi 或 TF 的管脚**（WiFi D2/D3 与 TF CMD/D0 被复用）——Application 1 = DVP+4线 SDIO WiFi（无 TF）；Application 2 = DVP+1线 SDIO WiFi + 1线 SDIO TF（IO 配置表原注）。

### 2.9 音频
- **功放**：**NS4150B（U8）** Class-D，输入 DAC1P/DAC1N（来自模组内部 DAC），输出 SPK_OUTP/SPK_OUTN → J7（MX1.25-2P 扬声器，默认 8Ω）；使能 **IOEX_AU_PA_EN**。R77/R78=43.2kΩ（增益/反馈），C29/C26/C31=330pF，C24=10µF，R87=0Ω。
- **麦克风**：双 PDM 数字 MEMS 麦克风 **MSM261DGT007**（MIC1/MIC2），PDM_DAT=**PA_55**、PDM_CLK=**PA_56**，使能 **IOEX_PDM_PWREN**，由 U12（3.3V LDO）供电；R90/R91=100Ω，C49=4.7µF，C50/C52/C51 退耦。

### 2.10 无线（模组内 AIC8800D40LS-N）
- 模组内 Wi-Fi/BT Combo 经 SDIO 接 MCU **PA_00–PA_05 + PA_45–PA_50**；天线座 **JP1（U.FL）**；**WS7872DE-6/TR（U6）** 为 RF 开关（2.4G/5G 切换，RF1/RF2，Mode VC），FL1（FLT18D24254959D-3288A）为 2.4G 滤波。
- 原注：**蓝牙 + 5G WiFi 可同时工作**；**蓝牙 + 2.4G WiFi 为硬切换、不能同时工作，需开机配置为 1**。

### 2.11 IO 扩展器（AW9523BTQR）
- **U11 AW9523BTQR** 经 I2C（PA22 SCL / PA23 SDA）提供 **16 路可控 IO**（P0_0–P0_7, P1_0–P1_7），用于板级外设使能/检测：IOEX_LCD_RST、IOEX_CTP_RST、IOEX_DVP_PDN、IOEX_WF_PWRKEY、IOEX_WF_PWREN、IOEX_2.4G_RFSW、IOEX_HST_WAK_WF、IOEX_AU_PA_EN、IOEX_PDM_PWREN、IOEX_LED_R/G/B、IOEX_TF_DET、IOEX_USB_DET。
- P1 为推挽（需软件设 1/4 驱动电流），P0 默认开漏可配置推挽；待机电流 <0.1µA；R86=2.2kΩ，R85=0Ω，C48=1µF。

### 2.12 LED 与用户指示
- 3 路用户 LED（红/绿/蓝，LED1/2/3 0603）由 **IOEX_LED_R/G/B** 经 R3/R4/R5（2.2kΩ）驱动。
- **WS2812B-2020-V6**（单线 RGB LED）接 **PA_08（RGB_LED）**，**与 TF 的 CLK（PA_08/SD1_CLK）复用**（V1.1 由 PA11 改到 PA08）。

### 2.13 40P 树莓派兼容排针（J3）
- **B-2100S40P-B110** 40 针，引出 3V3/5V/GND 及 GPIO（IO0–IO27 映射到 MCU PA 系列）、I2C2_SDA/SCL、SWCLK/SWDIO，供扩展与外设评估。

## 三、引脚与 GPIO 分配（依据 IO 配置表 / 原理图）
整板 **58 个 MCU GPIO（PA00–PA57）** 的完整分配见 [[raw/dev_board/gpio_table/57x LCD-DevKit ioconfig-11022026.xlsx]]（含复用 F1–F15、可用封装、关键网络、备注）。下表为各主要接口的引脚汇总：

| 接口 | MCU 引脚 | 网络/连接 | 备注 |
|---|---|---|---|
| RGB888 显示 R0–R7 | PA24 PA25 PA26 PA27 PA28 PA29 PA30 PA31 | DPI_R0…R7 | **仅 QFN80/BGA112 封装可用**（见封装约束） |
| RGB888 显示 G0–G7 | PA32 PA33 PA37 PA38 PA39 PA40 PA41 PA42 | DPI_G0…G7 | 同上 |
| RGB888 显示 B0–B7 | PA43 PA44 PA45 PA52 PA53 PA54 PA46 PA47 | DPI_B0…B7 | PA45–PA57 仅 QFN80/BGA112 |
| RGB 控制 | PA48(CLK) PA49(HSYNC) PA50(DE) PA51(VSYNC) | — | — |
| 背光 PWM | PA11 | BL_PWM | — |
| DVP 摄像头 | PA00–PA07, PA24–PA26, PA32 PA33 PA43 PA44 PA45 | DVP_* / 各数据线 | 经 DCMI MUX 跳线复用 WiFi/TF 脚 |
| SWD 调试 | PA43(SWCLK) PA44(SWDIO) PA20(SWO) | — | 与 DBUART 切换 |
| DBG UART | PA18(RXD) PA19(TXD) | DBG_UART1 | 下载/调试默认口 |
| USB2.0 FS | PA35(DP) PA36(DM) | USB_DP/DM | 设备口 |
| I2C（IO 扩展器） | PA22(SCL) PA23(SDA) | I2C_SCL/SDA | 与 32.768k 晶振复用 |
| GPADC 按键 | PA28–PA38（CH0–CH10）；PA34(PWRKEY) | GPADC_CHx | 3 路按键 + 电源键 |
| WS2812B | PA08 | RGB_LED | 与 TF_CLK 复用（V1.1 由 PA11 改） |
| LED（扩展） | IOEX_LED_R/G/B（U11） | — | 3 路用户 LED |
| Wi-Fi SDIO | PA00–PA05 + PA45–PA50 | WiFi_SD2_* | 模组内 AIC8800 |
| PDM 麦克风 | PA55(DAT) PA56(CLK) | PDM_DAT/CLK | MICBIAS 兼 |
| 音频 DAC | DAC1P/DAC1N（模组脚） | → NS4150B | 模拟输出 |
| TF 卡 | PA09(CMD) PA10(D0) PA08(CLK) | SD1_* | IOEX_TF_DET 检卡 |

> **封装约束**：PA45–PA57（含全部 RGB888 数据/控制脚）**仅 QFN80、BGA112 封装可用**，QFN68 无这些脚 → 本板 RGB888 必须选用 QFN80/BGA112 封装模组（与 MOD-WD 实际封装一致）。

## 四、物料清单要点（依据 BOM）
- **开发板底板 BOM 未单独提供**（本次仅导入模组 BOM，见 [[SF32LB57-MOD-WD_SCH_2026-08-26]]）。底板关键 IC 据原理图提取：ETA3425D2G×2（U4/U7 3.3V Buck）、NS4150B（U8 功放）、AW9523BTQR（U11 IO 扩展器）、CH343P（U3 USB-UART）、ME6211C15M5G-N（U10 1.5V LDO）、ETA5055V280DS2F（U9 2.8V LDO）、ETA5055V330DS2F（U12 3.3V LDO）、WS2812B-2020-V6（单线 RGB LED）、MSM261DGT007×2（PDM 麦克风）、AO3401A（Q1 电源 MOSFET）、BAT54WS×2（D9/D10）、SMF16CA（D4/D8 TVS）、各类阻容/磁珠/晶体。
- **模组 BOM（N16R8 / R16N16）**：见模组页「四、物料清单要点」。

## 五、关联页面
- 开发板实体：[[SF32LB57-DevKit-LCD]]
- 模组图纸：[[SF32LB57-MOD-WD_SCH_2026-08-26]]
- 使用指南来源：[[SF32LB57-DevKit-LCD_UserGuide]]
- 平台/芯片：[[SF32LB57X]]（3.3V 系列，QFN80）、[[SF32LB57]]
- 外设规范：[[SF32LB57_Peripheral_Display]]、[[SF32LB57_Peripheral_GPIO]]、[[SF32LB57_Peripheral_USB]]、[[SF32LB57_Peripheral_Audio]]、[[SF32LB57_Peripheral_BLE_RF]]、[[SF32LB57_Peripheral_Memory]]、[[SF32LB57_Peripheral_Power]]、[[SF32LB57_Hardware_Design_Guide]]
- 硬件设计指南（量产以该文档为准）：[[SF32LB57_Hardware_Design_Guide]]

## 六、待核实问题
1. **底板 BOM 缺失**：本次仅提供模组 BOM，开发板底板独立 BOM 未提供，底板物料以上述原理图提取为准（待补正式 BOM 校验）。
2. **电池/充电检测分压映射**：R53/R55(1.1k/2.8k) 与 R79/R28(2MΩ) 构成的电压检测网络阈值（2.97/2.58/2.19V）与 SF32LB57 内置充电管理的具体映射关系待核实。
3. **模组 MCU 封装**：模组用 QFN80（SF32LB57EYBBN6 / GYD7N6），与整板 RGB888 需 QFN80/BGA112 的约束一致（已自洽）；BGA112 版本未出现在本开发板物料中。
4. **WS7872DE（U6）功能**：原理图标注为 RF 开关（RF1/RF2，Mode VC），BOM 未注明具体功能，已据原理图推断为 2.4G/5G 天线切换开关。
