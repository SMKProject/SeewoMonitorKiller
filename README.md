<h1 align="center">
  <br>
  <a href="https://killer.smkproject.org/" alt="logo" ><img src="https://avatars.githubusercontent.com/u/277038346?v=4&size=150" width="150"/></a>
  <br>
  SeewoMonitorKiller
  <br>
</h1>

<h4 align="center">针对希沃集控的反杀系统</h4>

> [!IMPORTANT]
> 本项目(SeewoMonitorKiller)基于GPL-v3协议开源
> 开源版本已发布，包含核心功能（完整版不开源）

SeewoMonitorKiller 是针对希沃希沃管家监控的反监控系统，最新版本 V1.6（已发布预览版本）

由于班主任时不时通过希沃的巡课系统（俗称监控）来查看上课情况，这对学生来说当然不是一个好消息。因此，本反监控系统就是监控希沃，并在有异常情况时立即报告。

> [!CAUTION]
>
> 本项目最后更新于 2026年04月19日，希沃管家相关行为机制可能已经更改，请在安全环境自行测试项目是否仍然有效，有相关问题欢迎提 Issue。
>
> 本项目基于 "SeewoMonitorSystem"（疑似删库），原项目地址：https://github.com/DengHanxu/SeewoMonitorSystem
>
> 如您使用本软件，则代表您已阅读并同意："因使用本软件而产生的任何后果作者均不承担"这一声明

> [!WARNING]
>
> 本项目的思路构造以及极小部分源码借鉴 SeewoMonitorSystem，由于 GPL-v3 协议有继承性，现基于 GPL-v3 开源。

---

## 原理

希沃巡课系统主要依赖 3 个程序：`media_capture.exe`、`screenCapture.exe`、`rtcRemoteDesktop.exe`，其中 `media_capture.exe` 负责获取摄像头数据。

因此，不断查询当前进程列表并检查是否有 `media_capture` 等进程，就能大致确定（[这种方法并不完全准确](#局限性)）是否有老师正在查看监控。

---

## 主要构成

- [SeewoMonitorKiller](#seewomonitorkiller)（不开源）
- [SeewoMonitorKiller-OpenSource](#seewomonitorkiller-opensource)（开源版本）

### SeewoMonitorKiller

SeewoMonitorKiller 整合 SeewoMonitorSystem 的部分代码，并在此基础上增加部分功能，负责以指示块和系统通知的形式监控进程及杀进程。

> [!IMPORTANT]
> 完整版不开源，仅提供打包成品。

### SeewoMonitorKiller-OpenSource

开源版本，包含核心功能，适合二次开发和学习。

---

## 功能介绍

### 完整版特性

- 进程监控：监控 screenCapture.exe、media_capture.exe、rtcRemoteDesktop.exe
- 彩色指示灯：橙/红/蓝三色灯显示各进程状态，绿色灯表示全部终止
- 进程终止：多种终止方法（9种）
- 系统通知：Windows 10/11 系统通知
- 托盘图标：带状态变化的系统托盘图标
- 快捷键支持：Alt+Shift+X 打开设置，Alt+Shift+C 退出
- 网络监控：网络流量检测
- 配置加密：安全的配置文件加密
- 开机自启动：可选的开机自启动功能
- 自动更新：远程更新检查
- 详细日志：完整的日志记录系统

### 开源版本特性

- 进程监控：监控 screenCapture.exe、media_capture.exe、rtcRemoteDesktop.exe
- 彩色指示灯：橙/红/蓝三色灯显示各进程状态，绿色灯表示全部终止
- 进程终止：仅使用 psutil 终止（1种方法）
- 托盘图标：最小化到系统托盘，包含设置、关于、退出菜单
- 检测间隔设置：可调整的检测间隔
- 配置文件：简化的加密存储设置
- 日志记录：基本的日志记录

---

# 版本介绍

## 完整版（不开源）

| 版本 | 状态 | 发布日期 |
|------|------|----------|
| V1.0 | 未公开 | - |
| V1.1 | 未公开 | - |
| V1.2 | 未公开 | - |
| V1.3 | 未公开 | - |
| V1.4 | 未公开 | - |
| V1.5 | 未公开 | - |
| V1.6 | 预览版 | 2026.05.01（预计） |

### 未公开版本详情

#### SeewoMonitorKiller V1.0

第一个版本，默认以约 `1 次/秒` 的速度检测 `media_capture.exe`、`screenCapture.exe`、`rtcRemoteDesktop.exe`。

当检测到任一程序运行时，认为监控系统正在运行，并在屏幕上方正中的位置（已修复 SeewoMonitorSystem 显示偏左的问题）显示一个 4x4 像素的红/橙/蓝色方块。

当所有被监控程序结束运行时，认为本次监控结束，对应方块消失。

> [!IMPORTANT]
> 当前版本已不建议使用，故没有上传。检测延迟比 [SeewoMonitorSystem](https://github.com/DengHanxu/SeewoMonitorSystem) 多 ≥1.5s，经测试延迟 0.6s-1.5s 才会显示色块且并不显眼。

> [!IMPORTANT]
> 此程序已在代码中修改提权方式，不必像 [SeewoMonitorSystem](https://github.com/DengHanxu/SeewoMonitorSystem) 一样调用 `Nsudo.exe` 才能发挥作用，但为了使进程查杀完全生效，仍建议通过Nsudo启动
> 有关 `Nsudo.exe`，请参阅 [NSudo](https://github.com/M2TeamArchived/NSudo)。

#### SeewoMonitorKiller V1.1

第二个版本，增加了以下自定义设置：
- 检测进程时间
- 低网络检测时间
- 低网络关闭阈值
- 被监控程序被本程序关闭（低网络情况下）的时间
- 是否同意持续低网络（用户阈值网络使用大小）时间达到用户阈值时间时杀掉程序

> [!IMPORTANT]
> 当前版本已不建议使用，故没有上传。

> [!IMPORTANT]
> 当前版本与 V1.0 版本在初始化时均需 3 秒以上，且顶部绿灯在检测到右侧程序运行 1 次后不再亮起（暂未确定原因，故在 V1.2 及之后的版本修改了绿灯的逻辑）。

#### SeewoMonitorKiller V1.2

第三个版本，大版本更新：
- 修复低网络时间检测无法生效的问题
- 优化通知方案，降低监测误差
- 增加"关闭通知"选项用于关闭系统通知
- 增加错误处理机制

> [!IMPORTANT]
> 在该版本中"关闭通知"无法生效。

#### SeewoMonitorKiller V1.3

第四个版本：
- 更换数组，使关于中自定义监测时间可为小数
- 进一步降低监测延迟
- 优化其他方面，使代码容错率更高
- 将"关闭通知"改为"静音通知"

> [!IMPORTANT]
> "静音通知"仅为预期目标，实践证明无法实现，除非使用 plyer 模块通知（本版本仍使用 win10toast 模块）。

#### SeewoMonitorKiller V1.4

第五个版本，大版本更新：
- 改回"关闭通知"
- 修复每个通知都会在任务栏生成一个图标的问题
- 增加本程序任务栏图标显示，右键可打开"关于"或退出
- 优化关闭程序方案，使本程序在使用快捷键关闭或右键图标关闭时能够正常关闭
- 增加被监控程序的强制关闭方案，用两个方案同时在用户已设置的情况下关闭被监控程序（无需提权）
- 降低关于页面打开时间

> [!IMPORTANT]
> "静音通知"仅为预期目标，实践证明无法实现，除非使用 plyer 模块通知（本版本仍使用 win10toast 模块）。

#### SeewoMonitorKiller V1.5

过渡版本，优化进程监测与查杀

#### SeewoMonitorKiller V1.6

目前已处于预览计划中，预计2026.05.01发布正式版

> [!IMPORTANT]
> 完整版不开源，仅提供打包成品。

## 开源版本

| 版本 | 状态 | 发布日期 |
|------|------|----------|
| 260419V1 | 已发布 | 2026.04.19 |

### SeewoMonitorKiller-260419V1

开源版本，基于 V1.6 核心功能，已发布。

**主要特性**：
- 进程监控：监控 screenCapture.exe、media_capture.exe、rtcRemoteDesktop.exe
- 彩色指示灯：橙/红/蓝三色灯显示各进程状态，绿色灯表示全部终止
- 进程终止：仅使用 psutil 终止（1种方法）
- 托盘图标：最小化到系统托盘，包含设置、关于、退出菜单
- 检测间隔设置：可调整的检测间隔
- 配置文件：简化的加密存储设置
- 日志记录：基本的日志记录

**使用说明**：
1. 安装依赖：`pip install psutil pystray Pillow`
2. 运行：`python OpenSource/SeewoMonitorKiller-260419V1.py`
3. 打包：`pyinstaller --onefile --noconsole --name SeewoMonitorKiller-OpenSource OpenSource/SeewoMonitorKiller-260419V1.py`

---

## 项目结构

```
项目仓库/
├── FVersion/                  # 完整版（不开源）
│   ├── SeewoMonitorKiller.exe   # 完整版主程序
│   ├── black.ico                # 系统白天模式图标
│   └── white.ico                # 系统黑暗模式图标
├── OpenSource/                   # 开源版本
│   └── SeewoMonitorKiller-260419V1.py  # 开源版源码（260419V1 表示 2026年04月19日第1版）
├── README.md                     # 项目说明
└── requirements.txt              # 依赖包
```

---

## 依赖项

### 开源版本依赖
- tkinter
- psutil
- pystray
- Pillow

---

## 使用本项目

### 运行开源版本

1. 安装依赖：
   ```bash
   pip install psutil pystray Pillow
   ```

2. 运行开源版本：
   ```bash
   python OpenSource/SeewoMonitorKiller-260419V1.py
   ```

### 打包开源版本成 exe

1. 安装 PyInstaller：
   ```bash
   pip install pyinstaller
   ```

2. 打包命令：
   ```bash
   pyinstaller --onefile --noconsole --name SeewoMonitorKiller-OpenSource OpenSource/SeewoMonitorKiller-260419V1.py
   ```

### 使用完整版

> [!IMPORTANT]
> 完整版不开源，仅提供打包成品。请从官方渠道获取。

### 添加自启动（可选）

> [!IMPORTANT]
> 计划功能，正在规划中

---

## 局限性

[原理](#原理)中提到的 3 个程序有时会自己启动，导致误报，不过在用户于关于中设置的阈值时间后会被清除

目前尚不清楚相关机制

> [!IMPORTANT]
> 当前（截止 V1.6）尚未测试关于网络计量的相关逻辑是否能够正常运行，故推荐在关于中关闭"允许杀进程"选项以防万一。

---

## 贡献指南

1. Fork 本项目
2. 创建功能分支 (`git checkout -b feature/AmazingFeature`)
3. 提交更改 (`git commit -m 'Add some AmazingFeature'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 打开 Pull Request

---

## 许可证

本项目基于 [GPL-v3](LICENSE) 许可证开源。

---

## 免责声明

- 本项目仅供学习和研究使用
- 请勿将本项目用于任何违法或不当用途
- 因使用本项目产生的任何后果，作者不承担责任
- 请在遵守相关法律法规的前提下使用本项目

---

## 联系方式

- GitHub: [SeewoMonitorKiller](https://github.com/SMKProject/SeewoMonitorKiller)
- 作者: SMKProject
