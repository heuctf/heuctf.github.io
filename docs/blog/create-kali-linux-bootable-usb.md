---
title: 创建 Kali Linux 可启动 USB（三种不同方法）
date: 2026-04-09
tags:
  - Linux
  - 渗透测试
  - 环境配置
---

# 创建 Kali Linux 可启动 USB（三种不同方法）

Kali Linux 是渗透测试和安全审计领域最主流的 Linux 发行版，内置超 600 款开源安全工具。本文提供跨平台的 Kali Linux 可启动 U 盘制作指南，覆盖 Windows/Linux/macOS 三大系统，满足「实时运行」和「系统安装」两类核心场景。

## 一、为什么选择 Kali Live 实时 U 盘？
相比直接安装系统，Kali Live 可启动 U 盘具备以下优势：
- **非破坏性**：完全不修改主机硬盘数据和现有操作系统
- **自定义性**：支持定制专属 Kali 镜像，按需预装工具/配置
- **便携性**：可在任意 x86 设备上即插即用，无需携带专用电脑
- **持久化**：可配置持久分区，将操作数据、工具配置保存到 U 盘

> ⚠️ 重要提示：Kali 2020.1 及以上版本需**分开下载 Installer ISO** 和 **Live ISO**，旧版「单镜像包含双模式」的设计已取消。

<div align="center">
  <img src="@/images/kali-usb-guide/kali-live-advantage.png" alt="Kali Live 优势对比" width="700">
  <p>图1 Kali Live 实时 U 盘核心优势</p>
</div>

## 二、制作方法（按平台选择）
### 方法 1：Balena Etcher（跨平台通用）
Balena Etcher 是开源免费的镜像写入工具，界面极简，适合所有平台用户。

#### 操作步骤
1. 下载工具：[https://www.balena.io/etcher/](sslocal://flow/file_open?url=https%3A%2F%2Fwww.balena.io%2Fetcher%2F&flow_extra=eyJsaW5rX3R5cGUiOiJjb2RlX2ludGVycHJldGVyIn0=)
   - Linux 用户可直接运行 AppImage（无需安装）
   - Windows/macOS 下载对应安装包并启动

<div align="center">
  <img src="@/images/kali-usb-guide/etcher-main.png" alt="Balena Etcher 主界面" width="600">
  <p>图2 Balena Etcher 操作主界面</p>
</div>

2. 选择镜像：点击「Flash from file」，导入下载好的 Kali Linux ISO 文件（推荐 Live ISO）
3. 选择设备：点击「Select target」，选中目标 U 盘（**务必确认设备，避免误写硬盘**）
4. 开始写入：点击「Flash」，等待写入完成（耗时取决于 U 盘读写速度）
5. 完成使用：写入成功后会显示「Complete!」，直接弹出 U 盘即可

<div align="center">
  <img src="@/images/kali-usb-guide/etcher-flash.png" alt="Etcher 写入进度" width="600">
  <p>图3 Balena Etcher 镜像写入中</p>
</div>

### 方法 2：Rufus（仅 Windows）
Rufus 是 Windows 专属的开源工具，功能更丰富，支持持久分区配置。

#### 操作步骤
1. 下载工具：[https://rufus.ie/zh/](sslocal://flow/file_open?url=https%3A%2F%2Frufus.ie%2Fzh%2F&flow_extra=eyJsaW5rX3R5cGUiOiJjb2RlX2ludGVycHJldGVyIn0=)（免安装，双击 exe 启动）

<div align="center">
  <img src="@/images/kali-usb-guide/rufus-config.png" alt="Rufus 配置界面" width="600">
  <p>图4 Rufus 核心参数配置界面</p>
</div>

2. 核心配置（其余保持默认）：
   - **Device**：下拉选择目标 U 盘
   - **Boot selection**：选择「Disk or ISO image」，点击「SELECT」导入 Kali ISO
   - **Persistent partition size**：如需持久化，设置分区大小
   - **Partition Scheme & Target system**：
     - Legacy BIOS → 选 **MBR**
     - UEFI → 选 **GPT**
3. 开始写入：点击「开始」，等待完成。
4. 完成使用：状态显示「Ready」后关闭即可。

### 方法 3：DD 命令（Linux/macOS 终端）
适合习惯终端操作的用户，原生命令无需额外安装。

#### 操作步骤
1. 确认 U 盘设备路径：
   先拔出其他 U 盘，仅保留目标 U 盘，执行以下命令：
   ```bash
   sudo fdisk -l
