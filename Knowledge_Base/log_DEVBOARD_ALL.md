---
title: log_DEVBOARD_ALL
type: changelog
aliases: ["开发板知识库变更日志","开发板运维记录"]
tags: ["开发板","变更日志","仅追加"]
created: 2026-08-12
updated: 2026-08-12
---

# 企业开发板（Dev Board）硬件知识库 —— 变更日志（仅追加写入）

> **规则**：本文件只能从尾部追加，禁止修改/删除既有记录。
> **固定格式**：`YYYY-MM-DD | 资料来源 | 新建页面列表 | 更新页面列表 | 人工待确认事项`

---

## 变更记录

2026-08-12 | 初始化搭建（Agent 自动执行，基于用户要求"开发板与MCU平级大类"） | 新建：index_DEVBOARD_ALL.md、log_DEVBOARD_ALL.md；raw/dev_board/（schematics/pcb/bom/gpio_table/guide 子目录 + README）；wiki/dev_board/all/ 下 3 份 _template.md（sources/board_entity/schematics）；cmd/新增开发板入库指令.md、cmd/更新开发板入库指令.md | 无（首次初始化） | 待用户放入首份开发板原理图（建议 raw/dev_board/schematics/）；待核实开发板与 MCU 知识库双向关联入口是否已满足需求
2026-11-03 | 导入资料：开发板使用指南 `raw/dev_board/guide/SF32LB57-DevKit-LCD.md`（+10 张配图 assets/） | 新建页面：wiki/dev_board/all/sources/SF32LB57-DevKit-LCD_UserGuide.md（指南忠实全文，含 10 图嵌入、9 表、2 提示框）、wiki/dev_board/all/board_entity/SF32LB57-DevKit-LCD.md（11 项核心参数） | 更新页面：index_DEVBOARD_ALL.md（登记已入库开发板）、log_DEVBOARD_ALL.md | 待人工确认：①图纸（原理图/PCB/BOM/GPIO）缺失，实体页「图纸」项挂起，待补原理图建提取页；②硬件版本 V1.0/V1.1 未给单一标识；③模组 SF32LB57-MOD-WD 料号与合封 Flash/PSRAM 容量未列；④RGB888 全位宽依赖 QFN80/BGA112 模组封装（待核实）；⑤开发板演示电路≠量产标准，与硬件设计指南冲突以指南为准（已在来源页/实体页标注）

2026-11-03 | 补充图纸：底板原理图 V1.1 PDF（13页）、MOD-WD 模组原理图 PDF（3页）+ N16R8/R16N16 双 BOM xlsx、整板 IO 配置表 xlsx（均 raw 只读归档，源文件未改） | 新建页面：wiki/dev_board/all/schematics/SF32LB57-DevKit-LCD_SCH_V1_1.md（底板 V1.1：电源树/复位/时钟/BOOT/调试/存储/显示/摄像头/音频/无线/IO扩展器/LED/40P排针 + 整板 GPIO 分配）、wiki/dev_board/all/schematics/SF32LB57-MOD-WD_SCH_2026-08-26.md（模组：MCU/外置存储/时钟/无线/音频 + 双 BOM 对比 N16R8 vs R16N16）；raw 归档：raw/dev_board/schematics/(2 PDF)、raw/dev_board/bom/(2 xlsx)、raw/dev_board/gpio_table/(1 xlsx） | 更新页面：wiki/dev_board/all/board_entity/SF32LB57-DevKit-LCD.md（drawings 闭环、硬件版本确认 V1.1、模组料号/存储明确、§十一图纸填充、待核实 #1/#3 闭环）、index_DEVBOARD_ALL.md（登记图纸提取页 + 双 BOM + IO 配置） | 待人工确认：①开发板底板独立 BOM 未提供（以原理图提取为准）；②WS7872DE(U6)功能据原理图推断为 RF 开关、BOM 未注明；③N16R8/R16N16 存储映射为基于料号与命名的推断，以 datasheet 为准；④底板 PCB 文件未提供；⑤电池/充电检测分压映射（2.97/2.58/2.19V）待核实
2026-11-19 | 更新资料：SiFli Wiki 源站最新版 `SF32LB57-DevKit-LCD.md`（源文件只读，git 最新提交；旧 raw 版备份为 `..._V1_20261103_old.md`） | 新增 raw 配图：3 张（57DEVKIT-LCD-QSPI-Panel-Install / FPC_57_40p_to_RGB565软排线 / FPC_57_40p_to_SFLCM1p85软排线）+ FPC-REF.png（跨 sf32lb52x 目录引用） | 更新页面：`sources/SF32LB57-DevKit-LCD_UserGuide.md` 按源文件全量重建（必要硬件 3 条提示；模组 72 脚、40P DPI-RGB、24P DCMI 三表改为完整逐脚；WS2812B V1.1=PA_08；新增 DPI-RGB/QSPI 显示接口小节与排线下载链接；共 14 图）；`board_entity/SF32LB57-DevKit-LCD.md`（WS2812B 引脚统一 PA_08）；`index_DEVBOARD_ALL.md` | 待人工确认：①「相关文档」URL 按源文件恢复为 `docs/product-index.html`（旧知识库页曾为 `silicon/product-index.html#sf32lb57x`，若新 URL 有效可再更新）；②新增逐脚接口表线序需硬件复核；③板级单一版本标识仍缺
2026-11-19 | 用户拍板「按最新内容处理」| 无新建 | 更新页面：wiki/dev_board/all/sources/SF32LB57-DevKit-LCD_UserGuide.md「相关文档」段——规格书/芯片简介 URL 采用 `wiki.sifli.com/silicon/product-index.html#sf32lb57x`（替代源文件旧的 `docs/product-index.html`）、用户手册采用 `downloads.sifli.com/user%20manual/`（标注待发布）；保留源站当前页两个硬件资料 zip 链接 | 待人工确认：前一条 2026-11-19 记录的「待确认①相关文档 URL」已闭环；其余逐脚表线序复核、板级单一版本标识仍待确认
2026-11-19 | 用户更正「按这次更新文件里的内容处理」| 无新建 | 更新页面：wiki/dev_board/all/sources/SF32LB57-DevKit-LCD_UserGuide.md「相关文档」段——撤回上一条决策，规格书/用户手册/芯片简介三个链接全部恢复为**本次更新源文件原文** `https://wiki.sifli.com/docs/product-index.html`；保留源站当前页两个硬件资料 zip 链接 | 后置验证：该段 7 个链接与源文件逐行比对 MATCH=True，`silicon/product-index.html`、`downloads.sifli.com/user%20manual` 残留均为 0 | 待人工确认：前两条 2026-11-19 的「待确认①相关文档 URL」以本条为最终结论、闭环；其余逐脚表线序复核、板级单一版本标识仍待确认
