---
title: OpenCore 配置详解
date: 2024-03-15 14:30:00
tags: [OpenCore, EFI, 教程]
categories: [黑苹果教程]
---

## ACPI

ACPI（Advanced Configuration and Power Interface）是 OpenCore 配置中的重要部分，主要用于修复电源管理和硬件兼容性问题。

### 常用 SSDT

- **SSDT-EC**：修复嵌入式控制器，解决笔记本无法休眠问题
- **SSDT-PLUG**：修复 CPU 电源管理
- **SSDT-PMC**：修复电源管理控制器
- **SSDT-AWAC**：禁用 AWAC 时钟，强制使用 RTC

## DeviceProperties

DeviceProperties 用于向特定设备注入属性，最常见的是显卡和声卡。

### 显卡注入

```xml
<key>PciRoot(0x0)/Pci(0x2,0x0)</key>
<dict>
    <key>AAPL,ig-platform-id</key>
    <data>BgAmCg==</data>
    <key>framebuffer-patch-enable</key>
    <data>AQAAAA==</data>
    <key>framebuffer-cursormem</key>
    <data>AACQAA==</data>
</dict>
```

### 声卡注入

```xml
<key>PciRoot(0x0)/Pci(0x1F,0x3)</key>
<dict>
    <key>layout-id</key>
    <data>AQAAAA==</data>
</dict>
```

## Kernel

内核补丁是 OpenCore 的核心功能，用于修复系统启动和运行时的各种问题。

### 必要的 Kext

- **Lilu.kext**：各种补丁的通用平台
- **WhateverGreen.kext**：显卡补丁
- **AppleALC.kext**：声卡驱动
- **VoodooPS2Controller.kext**：键盘触摸板驱动
- **AirportBrcmFixup.kext**：博通无线网卡补丁

## quirks

Quirks 是 OpenCore 的特殊设置，用于绕过某些系统限制。

### 常用 Quirks

- **AppleXcpmCfgLock**：禁用 CFG Lock
- **DisableIoMapper**：禁用 IO Mapper
- **LapicKernelPanic**：修复内核崩溃
- **PatchPbiLR__OsxAptioFix3**：修复内存分配问题

## 总结

OpenCore 配置是一个复杂的过程，需要根据具体硬件进行调整。建议参考官方文档和社区资源进行配置。

> 参考：[OpenCore 官方文档](https://dortania.github.io/OpenCore-Install-Guide/)
