# 本次发布的验证范围

更新日期：2026-10-30（北京时间）。

## 实机证据

- Tahoe 26.7.1（25G241），WhateverXe 0.6.0 + Graphics 0.3.1。运行报告显示 Intel Iris Xe、1920×1080 60 Hz、Metal 2；机主确认显卡已正常驱动。独立性能基准和长期稳定性未完成。
- 触控板使用官方 VoodooI2C CI `6be653a2`。原始 artifact 摘要与 GitHub 元数据匹配，五个相关可执行组件均为 Intel x86_64 Mach-O。
- 重启报告中 I2C / GPIO / HID / VoodooInput 全部加载，ELAN2205 已绑定 PrecisionTouchpad 驱动，当前为 Polling 模式。机主确认移动、点击和滚动正常，随后反馈整体操作不够流畅。
- 三指拖移在采集时关闭；其他手势未逐项测试。不能据“Magic Trackpad 2”模拟名称宣称完整手势或力度触控正常。
- Wi-Fi 预设网络可自动连接；报告中 itlwm 已加载，HeliPort 未运行。手动选网流程待确认。
- 蓝牙控制器已识别，CD20-R 可被发现，日志显示 BLE 链路建立后停在 PHY 等待。配对仍未解决。
- 睡眠可进入但唤醒黑屏；未重复触发，也没有归因到确定的单一组件。

## 公开文件与隐私

2026-10-27 首次从成功启动 U 盘只读复制 149 个 EFI 文件。公开版剔除两份含私人信息的旧配置备份，保留 147 个文件，并清空 SMBIOS 序列号 / MLB / UUID / ROM 与 itlwm 自动连接信息。

本次仅更新 VoodooI2C.kext 与 VoodooI2CHID.kext。显卡、蓝牙、ACPI、正常配置、无 3D 配置、SIP 和轮询参数保持不变。两份公开配置通过 OpenCore 1.0.9 ocvalidate 检查；Kernel Add 的相关路径均存在，重复 PS2 VoodooInput 仍禁用。

发布前重扫个人身份值、Wi-Fi 密码、原始日志、采集报告和备份。原始运行报告仅本地保存，未上传 GitHub。公开模板需要读者自行填写独立 SMBIOS，未替读者作开机验证。
