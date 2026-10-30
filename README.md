# Huawei MateBook B3-420 Hackintosh — macOS Tahoe

华为 MateBook B3-420 的实验 EFI，基于实际启动 U 盘整理。**2026-10-30 更新：Iris Xe 已驱动，系统报告 Metal 2；ELAN2205 触控板已恢复移动、点击和滚动，但流畅度仍待改善，完整手势未逐项验证。睡眠和蓝牙仍未解决。**

Experimental EFI for Huawei MateBook B3-420 / i5-1135G7 / Iris Xe 9A49. Owner reports working graphics on Tahoe with WhateverXe 0.6.0 + Graphics 0.3.1. Trackpad movement, clicking and scrolling now work with the pinned upstream VoodooI2C CI build; smoothness and full gestures need further work. Wi-Fi auto-connect works with itlwm; manual network selection requires HeliPort and is not yet verified here. Bluetooth pairing and system sleep/wake remain unresolved.

## 硬件与版本

| 项目 | 本机配置 |
| --- | --- |
| 机型 / 主板 | Huawei MateBook B3-420 / NDZ-WXX9 / NDZ-WXX9-PCB-B3 |
| CPU | Intel Core i5-1135G7（Tiger Lake-U） |
| 核显 | Intel Iris Xe，PCI ID `8086:9A49` |
| 内存 | 16 GB DDR4 3200 MHz |
| 内屏 | BOE092E，1920 × 1080，eDP |
| 无线 / 蓝牙 | Intel AX201 |
| 内置 SSD | KPART512GBC2DVT，512 GB |
| BIOS | 2.31 |
| macOS | Tahoe 26.7.1（25G241） |
| OpenCore | 1.0.9 DEBUG；二进制与官方 DEBUG 发行包一致 |
| WhateverXe | 0.6.0 RELEASE |
| WhateverXe Graphics | 0.3.1；需另行安装，见下文 |
| SMBIOS 模型 | MacBookPro16,2；公开配置已清空个人身份字段 |

## 功能状态

| 功能 | 状态 / 证据范围 |
| --- | --- |
| 进入 Tahoe 桌面 | 机主实机确认 |
| Iris Xe 显卡驱动 / 图形加速 | 机主报告正常；运行报告确认 Metal 2；尚无独立性能基准或长期稳定性结论 |
| 触控板 | 移动、点击、滚动已实机确认；目前强制轮询，机主反馈操作不够流畅；其他手势待验证 |
| 睡眠 / 唤醒 | 机主报告仍不可用 |
| Wi-Fi | itlwm 预设网络自动连接可用；HeliPort 选网和重连流程待验证 |
| 蓝牙 | 控制器可识别、可发现鼠标；华为 CD20-R 配对卡在 PHY 等待；维护分支实验补丁未验证、未合入本 EFI |
| 音频、摄像头、亮度、键盘快捷键、电池、外接显示、USB 各端口 | 未逐项验证 |

这是已跑通显卡和触控板基础输入的阶段性记录，外围设备与交互体验仍在适配；不能视为功能完整的日用 EFI。同型号不同批次也可能使用不同触控板、无线或面板。

## 下载与使用

1. 从本仓库下载 EFI，并先备份自己的启动盘。**公开版必须先填写自己的 SMBIOS 身份信息。** 具体步骤见 [安装与恢复](docs/INSTALL.md)。
2. 当前路线需要 [WhateverXe Graphics 0.3.1](https://github.com/b00t0x/WhateverXe-Graphics/releases/tag/v0.3.1)。EFI 中的 WhateverXe 和 AMFIPass 不能替代 macOS 内的图形组件安装。
3. 如果图形组件尚未安装，先用 [无 3D 配置](configs/config-no3D.plist) 进入已有的 Tahoe，安装官方图形包后，再切换回启用 `-allow3d` 的配置。
4. 遇到黑屏时，使用无 3D 配置回退；操作方法见安装文档。先保留可用启动入口。

安装包直接从上游下载，本仓库不附带 macOS 安装介质、Apple 图形包或私人故障日志。

## 显卡路线记录

- 旧版 Graphics 0.1.0 / 0.2.0 在本机 Tahoe 上出现 `Xe compiler unsupported bitcode layout`，随后 WindowServer 崩溃。
- 上游 Graphics 0.3.1 带入系统原生 AIR 转换修复，并修正 0.3.0 的安装依赖范围。
- 本机安装日志确认官方 Graphics 0.3.1 成功安装；实际 U 盘确认 WhateverXe 0.6.0，随后机主反馈显卡正常。
- 显存容量、安装返回 0、驱动文件存在，均不单独作为 Metal 成功的证据；仍需补充渲染、视频和长时间稳定性测试。

保留的本机参数包括 `-allow3d -disablegfxfirmware -igfxdvmt`、IGPU DVMT 软件属性、现有 ACPI，以及 `csr-active-config=03080000`（0x803）。这个 SIP 值包含未认证根卷例外，降低系统完整性保护；AMFIPass 使用项目要求的 Mod 版本。未添加全局 AMFI 关闭参数。详见 [配置说明](docs/CONFIGURATION.md)。

## 触控板修复记录（2026-10-30）

本机为 `8086:A0E8` I2C 控制器 + `INT34C5` GPIO + `ELAN2205` I2C-HID。旧版匹配表不覆盖前两项，实际只加载了 VoodooI2CServices。

本次 EFI 更新为上游官方 CI 提交 [`6be653a2`](https://github.com/VoodooI2C/VoodooI2C/commit/6be653a2d2234ac1f7464ebf12716314975c3920) 的 **Release** 构建：`VoodooI2C.kext`（含 GPIO / Services / VoodooInput）与配套 `VoodooI2CHID.kext`。版本号仍显示 2.9.1，不能用旧稳定版 2.9.1 替代此构建。来源与摘要见 [触控板来源记录](docs/touchpad-source.json)。

重启后的运行报告确认 I2C、GPIO、HID、VoodooInput 均加载，ELAN2205 绑定到了 PrecisionTouchpad 驱动及 Magic Trackpad 2 模拟链路；机主确认移动、点击、滚动正常。

**当前限制：**保留原参数 `-vi2c-force-polling`，报告中运行模式为 Polling；机主反馈整体操作不够流畅。GPIO 中断模式尚未测试，本次不移除参数。三指拖移在采集时未开启，可在“系统设置 → 辅助功能 → 指针控制 → 触控板选项”开启；其他手势未逐项验证，不能据模拟设备名称宣称完整手势或力度触控可用。

蓝牙仅记录待解决状态。IntelBTPatcher 2.5.2 维护分支已准备为独立实验，尚无本机修复结果，本次不替换仓库中的 2.5.0。

## 隐私与来源

公开版已清空 SMBIOS 序列号、MLB、UUID、ROM，以及 itlwm 中的 Wi-Fi 自动连接配置。原始硬件采集、原始 EFI、安装日志和个人备份没有入库。原 U 盘保持原样。

[第三方组件与许可证](THIRD_PARTY.md) · [驱动版本清单](docs/kext-versions.json) · [文件摘要](SHA256SUMS) · [验证记录](docs/VALIDATION.md)

感谢 [b00t0x/WhateverXe](https://github.com/b00t0x/WhateverXe)、[WhateverXe-Graphics](https://github.com/b00t0x/WhateverXe-Graphics)、[JulioNectar](https://github.com/JulioNectar/Hackintosh-XPS13Plus-IrisXe-Tahoe)、Acidanthera、OpenIntelWireless、VoodooI2C、USBToolBox、Mieze、RehabMan 及相关项目作者。
