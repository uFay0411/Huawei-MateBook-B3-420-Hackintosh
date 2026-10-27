# 第三方组件与来源

本仓库原创文档和配置整理采用根目录 LICENSE。EFI 内第三方程序、固件及上游衍生内容保留各自许可；不统一重新许可为 MIT。许可证快照见 LICENSES。

| 组件 / 项目 | 官方源码或发布来源 |
| --- | --- |
| OpenCore / OpenRuntime / ResetNvramEntry | [acidanthera/OpenCorePkg](https://github.com/acidanthera/OpenCorePkg) |
| Lilu | [acidanthera/Lilu](https://github.com/acidanthera/Lilu) |
| VirtualSMC / SMC plugins | [acidanthera/VirtualSMC](https://github.com/acidanthera/VirtualSMC) |
| WhateverGreen（禁用） | [acidanthera/WhateverGreen](https://github.com/acidanthera/WhateverGreen) |
| WhateverXe | [b00t0x/WhateverXe](https://github.com/b00t0x/WhateverXe) |
| WhateverXe Graphics / AMFIPass Mod | [b00t0x/WhateverXe-Graphics](https://github.com/b00t0x/WhateverXe-Graphics) |
| BlueToolFixup | [acidanthera/BrcmPatchRAM](https://github.com/acidanthera/BrcmPatchRAM) |
| BrightnessKeys | [acidanthera/BrightnessKeys](https://github.com/acidanthera/BrightnessKeys) |
| NVMeFix | [acidanthera/NVMeFix](https://github.com/acidanthera/NVMeFix) |
| RestrictEvents | [acidanthera/RestrictEvents](https://github.com/acidanthera/RestrictEvents) |
| VoodooPS2Controller | [acidanthera/VoodooPS2](https://github.com/acidanthera/VoodooPS2) |
| VoodooI2C / HID / GPIO / Input | [VoodooI2C/VoodooI2C](https://github.com/VoodooI2C/VoodooI2C) |
| itlwm | [OpenIntelWireless/itlwm](https://github.com/OpenIntelWireless/itlwm) |
| IntelBluetoothFirmware / IntelBTPatcher | [OpenIntelWireless/IntelBluetoothFirmware](https://github.com/OpenIntelWireless/IntelBluetoothFirmware) |
| USBToolBox / UTBDefault | [USBToolBox/kext](https://github.com/USBToolBox/kext) |
| RealtekRTL8111 | [Mieze/RTL8111_driver_for_OS_X](https://github.com/Mieze/RTL8111_driver_for_OS_X) |
| XHCI-unsupported 相关来源 | [RehabMan/OS-X-USB-Inject-All](https://github.com/RehabMan/OS-X-USB-Inject-All) |
| HfsPlus 等 EFI 文件参考来源 | [acidanthera/OcBinaryData](https://github.com/acidanthera/OcBinaryData) |

依照版本清单查阅相应上游源代码与构建说明。除已单独核对的 OpenCore / WEX / AMFIPass，原启动盘其他二进制未完成逐一与上游发行包的重建对应；其摘要随本仓库保留。`apfs_aligned.efi` 和 SSDT 保留用户实际启动盘版本，当前未建立确切的上游提交对应，不声称为本仓库原创。

Graphics 0.3.1 pkg 不存放在本仓库；请直接从上游取得并遵循其组件声明。AMFIPass Mod 来源与哈希见下载来源文件。
