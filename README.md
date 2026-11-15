# Redmi Book 16 2024 · macOS 15.8.2 Run

i5-12450H / Intel UHD 48EU（`8086:46A3` rev 0x0C）/ macOS 15.8.2 (24H232) / OpenCore 1.0.8
本EFI和GPU配置亦可适用与主要受益者：同型号 GPU、卡在"8MB 无 Metal"的情况

## 本机配置：
| 项 | 值 |
|---|---|
| 机型 | Redmi Book 16 2024 / TM2303-53595 |
| CPU | **i5-12450H**（Alder Lake-P，4P+4E / 12 线程，906A3） |
| 核显 | **Intel UHD Graphics 48EU，`8086:46A3`，rev 0x0C** |
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
Redmi2024 MacOS GPU\
├── README.md                              本文件
├── EFI\                                   ✅ 完整可用 EFI（BOOT + OC，22 个 kext）
├── WhateverXe.kext                        成果二进制（可直接替换进 EFI）
├── wex-0.8.0-adlp-46a3-edp-coldcdclk.patch三处源码改动（相对官方 0.8.0，可直接 patch -p1）
```

## 怎么用

使用 *DiskGenius*  挂载ESP（右键ESP， 指派新的驱动器号（盘符）），随意设置即可
打开文件资源管理器， 你会看到一个200MB左右的硬盘， 双击该硬盘并双击EFI文件夹， 粘贴此EFI在该文件夹下即可

## 暂时无法使用的：

- 触摸板
- 摄像头
- 麦克风

未验证：HDMI / USB-C 外接、睡眠唤醒、视频硬解。
系统更新后**必须重装 WhateverXe Graphics 包**（`/Library` 里的驱动会被 AuxKC 飞起来）
