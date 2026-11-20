# Redmi Book 16 2024 · macOS 26（Tahoe）

i5-12450H / Intel UHD 48EU（`8086:46A3` rev 0x0C）/ macOS 26.7+ / OpenCore 1.0.8

本分支（`tahoe`）目标系统为 **macOS 26**，kext 全部换为 26 兼容版本；macOS 15.8.2 的可用快照见 `main` 分支。

# USBMap（USB 端口映射）

本 EFI 目前仍使用 `USBInjectAll` + `MacBookPro16,2` 机型端口表，与真机 2×USB-A + 2×USB-C 并不对应，建议先做一份 `USBMap.kext` 再使用。

- 工具：[USBToolBox](https://github.com/USBToolBox/tool)（Windows / macOS 均可运行）
- 生成后放入 `EFI/OC/Kexts`，在 `config.plist` 的 `Kernel → Add` 中启用 `USBMap.kext`、禁用 `USBInjectAll.kext`
- 具体制作方法待补充

## 本机配置

| 项 | 值 |
|---|---|
| 机型 | Redmi Book 16 2024 / TM2303-53595 |
| CPU | **i5-12450H**（Alder Lake-P，4P+4E / 12 线程，906A3） |
| 核显 | **Intel UHD Graphics 48EU，`8086:46A3`，rev 0x0C**  ⚠️ 不保证 26.7 以下可使用 |
| 内存 | 16 GB |
| 内屏 | MNG007QS4-3，**1920×1200 eDP** |
| 无线 | Intel AX203（`8086:51F0`） |
| 蓝牙 | Intel（USB，`8087:xxxx`） |
| 声卡 | Realtek ALC256（`alcid=5`） |
| 硬盘 | ZHITAI Ti600 NVMe |
| TPM | 有 TPM 2.0（BIOS 锁死且无法被关闭） |
| BIOS | RMAAD6B0P0606，Insyde H2O，**无 Advanced 页** |

## 文件夹内容

```
EFI\
├── BOOT\                                   BOOTx64.efi
└── OC\                                     OpenCore 1.0.8
    ├── ACPI\                               16 个 SSDT
    ├── Kexts\                              22 个
    ├── Drivers\                            HfsPlus、OpenRuntime
    └── config.plist
WhateverXe.kext                             成果二进制（可直接替换进 EFI\OC\Kexts）
```

## 怎么用

使用 *DiskGenius* 挂载 ESP（右键 ESP，指派新的驱动器号（盘符）），随意设置即可
打开文件资源管理器，你会看到一个 200MB 左右的硬盘，双击该硬盘并双击 EFI 文件夹，粘贴此 EFI 在该文件夹下即可

## 显卡加速

本机 GPU 的加速来自 WhateverXe。EFI 只负责让系统启动并点亮 framebuffer，真正的加速由 `WhateverXe Graphics` 包提供：

1. 装完 macOS 26.7 后安装 `WhateverXe Graphics` 包（它自行安装到 `/Library` 并重建 AuxKC，**不需要 OCLP**）
2. 重启，确认 `kextstat | grep -i TGLCompat` 有输出
3. 每次 26.x 小版本更新后重装该包（`/Library` 里的驱动会被 AuxKC 作废）

## 暂时无法使用的

- 触摸板
- 摄像头
- 麦克风：ALC256 的内建麦节点为 `0x12`，当前 `alcid=5` 未启用内建麦；`layout 13 / 56` 含该节点，`69` 为小米同系机型专用，可依次尝试，**待真机验证**

未验证：HDMI / USB-C 外接、睡眠唤醒、视频硬解。
