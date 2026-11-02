---
title: SF32LB57 安全与加速外设（AES/SM4·HASH·CRC·TRNG·efusec·CORDIC）
type: mcu_peripheral
aliases: ["SF32LB57_Peripheral_Security","SF32LB57安全","SF32LB57 AES","SF32LB57 efuse","SF32LB57 TRNG"]
tags: ["企业MCU","外设-安全/加速","芯片系列-SF32LB57","原理图审图/故障分析/报告输出"]
sources: [SF32LB57X_Datasheet_V0.2.2],[SF32LB57x_batt_Datasheet_V0.2.2],[UM5701-SF32LB57x-用户手册_V0p1]]
mcu_family: ["SF32LB57"]
series_applicability: ["通用-两系列共用"]
mcu_model: ["SF32LB57X","SF32LB57x"]
hardware_version: ["QFN68 7x7","QFN80 8x8","BGA112 5.4x4.4","UM5701 V0.1"]
created: 2026-11-02
updated: 2026-11-02
---

## 摘要
SF32LB57 安全与加速子系统**两系列一致**，包含：
- **AES_ACC 加解密/散列加速**：对称加密 **AES128/192/256、SM4**，模式 **ECB/CTR/CBC**；散列 **SHA1/SHA224/SHA256/SM3**。基地址 **0x5000D000**。
- **CRC**：7/8/16/32 位可配，任意多项式/初始值，字节/半字/字高低位倒转，每 HCLK 周期 1 字节。
- **TRNG**：真随机数发生器（熵源 + 种子发生器 + PRNG）。
- **efusec**：eFuse 控制器，4×256bit OTP（共 1024bit），可读写、bank0 可锁 bank1~3。
- **CORDIC 协处理器**：三角函数/双曲函数/算术加速（仅 HPSYS 集成 1 个，32 位定点）。

> 📌 与 datasheet「安全：AES / HASH / CRC / TRNG / efusec」及「eShield 安全架构（Flash 实时解密）」对应；AES 可选内部 **root key（来自 efuse `rootkey[255:0]`）** 作为密钥（`KEY_SEL=1`）。

## 1. AES_ACC 加解密与散列（UM §13.1，印刷 p358–363）
- **对称加密**：AES（128/192/256 由 `AES_LENGTH` 选）+ 国密 **SM4**（`ALGO_STANDARD=1`）。
- **模式**：ECB / CTR / CBC（由 `AES_MODE` 选）；CTR 常用于外部存储数据加解密（向量与地址相关，可并行）。
- **散列**：SHA-1（160bit）、SHA-224（224bit）、SHA-256 / SM3（256bit），由 `HASH_MODE` 选。
- **数据搬运**：启动后由内部 DMA 读写，输入/输出地址寄存器 `DMA_IN` / `DMA_OUT`，块大小 16 字节（`DMA_DATA.SIZE`）。
- **密钥**：`KEY_SEL=0` 用 `EXT_KEY_W0~7` 外部密钥；`KEY_SEL=1` 用 **efuse 内部 root key**。
- 基地址 **0x5000D000**；典型命令/状态/中断寄存器：`COMMAND`、`STATUS`、`IRQ`、`SETTING`、`AES_SETTING`、`IV_W0~3`、`EXT_KEY_W0~7`、`HASH_SETTING` 等。

## 2. CRC（UM §12.2，印刷 p354–357）
- 支持 **7/8/16/32 位** CRC，任意自定义多项式、任意初始值。
- 输入数据单元为字节，支持单/双/三/四字节有效位宽与字节/半字/字高低位倒转，输出可倒转。
- 计算速度：每 HCLK 周期 1 字节；数据全部输入后即时得结果。
- 内置主流算法配置表（CRC-7/MMC、CRC-8 系列、CRC-16 系列、CRC-32/MPEG-2 等）；模块不做结果异或，需软件后处理。
- 寄存器：`DR`(数据)、`SR`(状态 done/overflow)、`CR`(REV_OUT/REV_IN/POLYSIZE/DATASIZE/RESET)、`INIT`(初值)、`POL`(多项式)。

## 3. TRNG 真随机数发生器（UM §13.2，印刷 p364–368）
- 组成：熵源（6 条反相器链）→ 随机种子发生器（CASR 元胞自动机移位寄存器）→ 伪随机数发生器（PRNG）。
- 熵源每次生成种子时启动；`reject_threshold`(VN 校验器门限) 越高筛选越宽松、生成越快但随机性下降。
- 支持 `use_ext_seed=1` 使用外部种子；`gen_seed_start` / `gen_rand_num_start` 分别触发。
- ⚠️ **PRNG 可能进入自锁（prng_lockup）**（LFSR 死锁），检测到 `prng_lockup` 状态建议重新生成种子。
- 附带反相器链震荡周期测量，可由 `Fpclk`、`Npclk`、`Ninv` 推算 `Finv` 以推断芯片工艺角（与硬件一致性/筛选弱相关）。

## 4. efusec eFuse 控制器（UM §13.3，印刷 p368–371）
- **容量**：4 块 × 256bit = **1024bit** OTP；熔断后 bit 读回为 1，**熔断不可逆（1 不能恢复为 0）**，未熔断 bit 可再次熔断。
- 读写为**单 bit 串行**，每次一个 efuse 单元；读写完成可产生中断；支持读写屏蔽。
- 接口输出信号：`idsel`、`swddis`、`pkgid[1:0]`、`uid[127:0]`、**`rootkey[255:0]`**（即 AES 内部 root key 来源）。
- **读写屏蔽（安全）**：bank0 固定 bit 可屏蔽 bank1~3 的读/写（如 `bank0[255:254]=2'b11` 屏蔽 bank3 写、`bank0[253:252]=2'b11` 屏蔽 bank3 读，依此类推 bank2/bank1）。
- 时序：单 bit 熔断 TCKHP≈10µs；THPCK（写保持）>20ns；THRCK（读保持）>500ns；`TIMR` 寄存器按工作时钟计数值配置（复位值按 48MHz 给出）。
- 控制流程：配置 `BANKSEL` → `MODE`(0 读/1 编程) → `IE` → `PGM_DATA*`/`TIMER` → `EN` 自清零启动 → 中断/SR 等待。

## 5. CORDIC 协处理器（UM §12.1，印刷 p354）
- 用于三角函数（cos/sin/atan/rot/mod/ang）与双曲函数（cosh/sinh/atanh/… /mul/div/ln/exp/sqrt）及推演算术。
- 支持 ARM 协处理器指令与 Custom Datapath Extension（仅 HPSYS）；**32 位定点输入/输出**。
- 仅在 **HPSYS 集成 1 个** CORDIC。

## 6. 硬件设计注意
- 上述模块均为**片上数字/安全逻辑**，无专用外部引脚与外围器件，原理图层面无连接要求。
- **efusec / rootkey 为安全与配置关键**：芯片配置位（如 `pkgid`、`uid`、安全启动 key）固化于 efuse，熔断不可逆；量产烧写与锁定位策略须谨慎（影响 Secure Boot / Flash 解密可用性）。
- AES `KEY_SEL=1` 内部 root key 来自 efuse，与外部密钥方案的选择涉及系统安全架构，需结合固件与 Secure Boot 设计。

## 关联页面
- 家族：[[SF32LB57]]｜3.3V：[[SF32LB57X]]｜电池：[[SF32LB57x_batt|SF32LB57x]]
- 存储（Flash 实时解密 eShield）：[[SF32LB57_Peripheral_Memory]]
- 来源：[[UM5701-SF32LB57x-用户手册_V0p1]]

## 原始来源完整引用
1. [UM5701-SF32LB57x-CN V0.1]，第 12 章 加速器（印刷 p354–357）：§12.1 CORDIC 协处理器（p354）、§12.2 CRC（p354–357）。
2. [UM5701-SF32LB57x-CN V0.1]，第 13 章 安全（印刷 p358–371）：§13.1 AES_ACC（AES/SM4 + HASH，p358–363，基地址 0x5000D000）、§13.2 TRNG（p364–368）、§13.3 efusec（p368–371）。

## 待核实问题
1. AES/TRNG/efusec/CRC 的**基地址**是否各为独立外设（UM 仅明示 AES=0x5000D000、GPADC=0x50087000、TSEN=0x50089000），TRNG/efusec/CRC 基地址待从 UM 地址映射总表补全。
2. efuse `rootkey[255:0]` 与 Secure Boot / Flash 实时解密（eShield）的绑定关系与烧写策略，UM 未细化，待官方安全启动文档。
3. CORDIC 仅在 HPSYS 集成，是否 LPSYS 不可用、与低功耗域关系待确认。
