# 当前配置

- SMBIOS 模型：`MacBookPro16,2`。身份字段已清空，下载后自行生成。
- OpenCore：1.0.9 DEBUG。调试模式和 verbose 参数来自实际成功启动配置。
- WhateverXe：0.6.0 RELEASE，Lilu 1.7.3，AMFIPass 1.4.1 Mod。
- Graphics：0.3.1；其 Common 包内 Compat 0.6.0 的兼容下限为 0.5.0，满足 Tahoe framebuffer 的 0.5.0 依赖。
- 保留实际成功使用的 `-allow3d -disablegfxfirmware -igfxdvmt`，以及现有 CPU/引导参数。
- IGPU 路径：`PciRoot(0x0)/Pci(0x2,0x0)`，Data 类型 `framebuffer-patch-enable=01000000`、`framebuffer-stolenmem=0000C003`；对应本机固定 60 MB DVMT 的软件处理。
- `csr-active-config` Data 为 `03080000`，数值 `0x803`：允许未签名 kext、受限文件系统例外及未认证根卷。它降低 SIP 保护，属于本实验图形路线的要求。
- WhateverGreen 仍保留在原 EFI 目录，但配置项为禁用；不能同时启用 WhateverGreen 和 WhateverXe。NullMoth NMX 不在当前激活配置中。
- Wi-Fi：itlwm 2.3.0，公开版 `WiFiConfig` 为空。包含驱动不等于选网、连接和重连功能已验证。
- 蓝牙与触控板驱动保留实际 U 盘状态，仍未解决；不把已有 kext 当成工作证明。
- USB 仍使用 USBToolBox + UTBDefault 等现有配置，没有发布本机完成端口定制的结论；睡眠问题也未定位。

全部 kext 的启用状态和版本见 `kext-versions.json`。ACPI 保留原启动盘的 11 张 SSDT；本次仅发布记录，没有为未解决设备追加未经验证的修复。
