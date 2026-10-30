# 安装与恢复

本说明记录已安装 Tahoe 26.7.1（25G241）上的显卡迁移路径。全新安装器、其他系统版本及其他机器需要另行验证。触控板基础输入已恢复，但流畅度和完整手势仍待完善；睡眠、蓝牙配对和 Wi-Fi 选网流程仍待验证。

## 准备公开 EFI

1. 备份现有 EFI，在独立 U 盘上测试。保留能启动 Windows / 原 macOS 的入口。
2. 使用 [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS) 等工具，为 `MacBookPro16,2` 生成自己独立的身份信息。
3. 在 `EFI/OC/config.plist` 的 `PlatformInfo > Generic` 中填写 `SystemSerialNumber`、`MLB`、`SystemUUID` 和自己的 6 字节 `ROM`。仓库中的空值和全零占位值需要替换。
4. 将完全相同的身份信息填入 `configs/config-no3D.plist`，随后使用与 OpenCore 1.0.9 匹配的 `ocvalidate` 验证两份配置。
5. 本仓库已删除 itlwm 预设 Wi-Fi 信息。Wi-Fi 当前仍未解决，不要依赖无线网络完成安装器下载。

## 图形组件

EFI 已含 WhateverXe 0.6.0、Lilu 和 AMFIPass 1.4.1 Mod。Graphics 0.3.1 是单独的 macOS 安装包，需要安装到当前运行的系统。

1. 下载官方 [WhateverXe-Graphics-0.3.1.pkg](https://github.com/b00t0x/WhateverXe-Graphics/releases/download/v0.3.1/WhateverXe-Graphics-0.3.1.pkg)。本机测试的 SHA-256：

   ```text
   bd8b449431b1bf3815f9011ad3e13c7038012d17aed54dfb2aecfce6b9089623
   ```

2. 图形组件未装好时，先将已填好身份信息的 `configs/config-no3D.plist` 复制为启动 U 盘的 `EFI/OC/config.plist`，再进入已有 Tahoe。不要启用旧的 WhateverGreen 或 NullMoth 图形栈来混搭。
3. 如已有其他项目写入 System 卷的旧图形补丁，先按原项目恢复流程处理。本机成功前的起点是无 System 图形补丁、官方 Graphics 0.2.0；迁移时使用上游 [uninstall.sh](https://github.com/b00t0x/WhateverXe-Graphics/blob/59e4ca9e474781dba80a5b5f60fba1767d0a9c2a/uninstall.sh) 移除旧组件，然后安装完整官方 0.3.1。
4. 安装成功、没有报错后，将填好身份信息的正常 3D 配置恢复到启动 U 盘，再手动重启，从同一 U 盘进入已安装系统。安装失败先保存错误，保留当前会话，不立即重启。
5. 检查系统信息中的 Metal 支持、窗口拖动、视频播放及实际 3D 渲染。仅有 1536 MB 显存或安装返回 0 不能证明硬件加速。

上游图形包将组件安装到 `/Library` 并更新 AuxKC；Tahoe 下会关闭 `CoreMedia / AllowOnePassMetalScaling`。普通上游 AMFIPass 与本路线的 Mod 签名链不同，不能直接替换。SIP 0x803 的保护变化见配置说明，使用前需要理解并自行决定是否接受。

## 黑屏或 WindowServer 循环

1. 关机，把测试 U 盘接到另一台电脑。
2. 备份 U 盘当前配置，将填好同一组身份信息的 `configs/config-no3D.plist` 复制为 `EFI/OC/config.plist`。这只移除 `-allow3d`，不卸载已安装组件。
3. 从该 U 盘重新进入 Tahoe。无 3D 模式用于恢复，界面没有硬件加速。
4. 如需卸载图形包，在能启动的 macOS 下按上游卸载说明处理；EFI 中的 WEX 和系统图形包是两部分，单独删除一个不能完整恢复。

本仓库不提供自动抹盘、内置盘 EFI 覆盖或 BIOS 修改脚本。BIOS 2.31 是硬件记录，并非要求其他机器刷入此版本。
