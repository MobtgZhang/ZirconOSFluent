# ZirconOSFluent - Windows 10 Fluent Design 桌面主题

## 概述

ZirconOSFluent 是 ZirconOS 操作系统的 **Windows 10 Fluent Design** 风格桌面环境实现。
Fluent Design System 是微软于 2017 年推出的设计语言，以 **光照（Light）、深度（Depth）、
动效（Motion）、材质（Material）、缩放（Scale）** 五大核心元素为基础，引入了亚克力半透明材质
和系统级暗色模式。

本模块参考 [ReactOS](https://github.com/reactos/reactos) 的桌面架构设计，
目标是实现 Windows 10 后期（20H2+）的 Fluent Design 桌面 Shell。

## 设计风格

### Fluent Design 核心视觉特征

| 特征 | 说明 |
|------|------|
| **亚克力材质 (Acrylic)** | 半透明磨砂玻璃效果，比 Aero 更朦胧现代 |
| **暗色模式 (Dark Mode)** | 系统级深色 UI，`#1F1F1F` 背景 |
| **Reveal 高亮** | 鼠标移动时控件边缘显示光照追踪效果 |
| **圆角 (4px)** | 窗口和控件使用小圆角 |
| **Segoe UI** | 默认字体 Segoe UI Variable 9pt |
| **动效系统** | 连接动画、页面过渡、隐式动画 |

### 配色方案

#### 亮色模式

| 元素 | 颜色值 | 说明 |
|------|--------|------|
| 窗口背景 | `#FFFFFF` | 白色 |
| 标题栏 | `#FFFFFF` / `#F3F3F3` | 白色/浅灰（沉浸式标题栏） |
| 任务栏 | `#F3F3F3`（实心）/ 亚克力半透明 | 浅灰色 |
| 强调色 | `#0078D4`（默认蓝色） | 可自定义 |
| 文字 | `#000000` / `#323130` | 黑色/深灰 |

#### 暗色模式

| 元素 | 颜色值 | 说明 |
|------|--------|------|
| 窗口背景 | `#1F1F1F` / `#2D2D2D` | 深灰色 |
| 标题栏 | `#1F1F1F` / `#2D2D2D` | 深灰色 |
| 任务栏 | `#1F1F1F`（实心）/ 亚克力半透明 | 深色 |
| 强调色 | `#0078D4`（默认蓝色） | 可自定义 |
| 文字 | `#FFFFFF` / `#E4E4E4` | 白色/浅灰 |

### 与其他主题的关键差异

- **开始菜单**：结合列表 + 动态磁贴（Live Tiles），支持全屏模式
- **任务栏**：支持亚克力透明/不透明切换，Cortana 搜索栏
- **通知中心**：右侧滑出面板（Action Center），包含快速设置
- **设置应用**：取代传统控制面板，UWP 风格
- **双模式**：同时支持亮色和暗色主题

## 模块架构

```
ZirconOSFluent/
├── src/
│   ├── root.zig              # 库入口，导出所有公共模块
│   ├── main.zig              # 可执行入口 / 集成测试
│   ├── theme.zig             # Fluent 主题定义（亚克力参数、强调色、暗色模式）
│   ├── winlogon.zig          # 用户登录管理（Win10 锁屏 + 登录界面）
│   ├── desktop.zig           # 桌面管理器（壁纸、虚拟桌面）
│   ├── taskbar.zig           # 任务栏（搜索框、任务视图、系统托盘）
│   ├── startmenu.zig         # 开始菜单（应用列表 + 动态磁贴）
│   ├── window_decorator.zig  # 窗口装饰器（沉浸式标题栏、Snap Assist）
│   ├── shell.zig             # 桌面 Shell 主程序（explorer.exe 风格）
│   └── controls.zig          # Fluent 风格控件（Reveal 效果、Toggle Switch）
├── resources/
│   ├── wallpapers/           # 桌面壁纸（Hero 壁纸、Spotlight）
│   ├── icons/                # 系统图标（Fluent 扁平化彩色图标）
│   ├── ui/                   # UI 组件素材
│   ├── cursors/              # 鼠标光标
│   └── MANIFEST.md           # 资源清单
├── build.zig
├── build.zig.zon
└── README.md
```

## 计划实现的组件

### WinLogon（用户登录）
- **锁屏界面**：全屏壁纸 + 时间/日期 + 上滑解锁
- **登录界面**：用户头像 + PIN/密码输入 + 辅助功能

### Desktop（桌面管理器）
- Hero 壁纸（Windows 10 标志性蓝色光线壁纸）
- 虚拟桌面（Win+Tab 多桌面切换）
- 桌面图标（Fluent 扁平化彩色图标）
- 右键菜单（圆角 + 阴影 + 亚克力背景）

### Taskbar（任务栏）
- **搜索框** / **Cortana 按钮**
- **任务视图按钮**（虚拟桌面 + 时间线）
- 固定应用图标 + 运行中应用指示器
- 系统托盘（可折叠、日历弹出窗口）
- **通知中心**（Action Center）按钮

### Start Menu（开始菜单）
- **左侧**：应用列表（字母索引 + 拼音首字母跳转）
- **右侧**：动态磁贴（可调整大小：小/中/宽/大）
- 电源按钮、设置、用户头像
- 支持全屏开始屏幕模式

### Window Decorator（窗口装饰器）
- 沉浸式标题栏（标题栏颜色跟随应用/系统配色）
- Snap Assist（拖拽吸附 + 推荐布局）
- 窗口阴影（圆角矩形投影）
- 标题栏按钮（最小化/最大化/关闭，悬停变色）

### Controls（UI 控件）
- Fluent Button（悬停 Reveal 光照效果）
- Toggle Switch（滑动开关）
- Slider（滑块）
- ComboBox（下拉框，亚克力弹出）
- ProgressRing（圆环进度指示器）
- InfoBar（信息提示条）

## 与主系统集成

ZirconOSFluent 通过以下内核子系统接口工作：

1. **user32.zig** — 窗口管理 API、消息队列
2. **gdi32.zig** — 绘图 API（需扩展亚克力材质渲染）
3. **subsystem.zig** (csrss) — 窗口站和桌面管理
4. **framebuffer.zig** — 帧缓冲区显示驱动

### 配置

在 `config/desktop.conf` 中选择 Fluent 主题：

```ini
[desktop]
theme = fluent
color_scheme = light      # light | dark
shell = explorer
```

## 构建

```bash
cd 3rdparty/ZirconOSFluent
zig build
zig build test
```

## 开发状态

当前为项目框架阶段，计划按以下顺序实现：

1. `theme.zig` — Fluent 配色、亚克力参数、暗色模式定义
2. `controls.zig` — Fluent 控件（Reveal 效果、Toggle Switch）
3. `window_decorator.zig` — 沉浸式标题栏
4. `taskbar.zig` — 任务栏（搜索框、任务视图）
5. `startmenu.zig` — 磁贴开始菜单
6. `desktop.zig` — 桌面管理器（虚拟桌面）
7. `winlogon.zig` — 锁屏 + 登录界面
8. `shell.zig` — Shell 集成

## 参考

- [ReactOS](https://github.com/reactos/reactos) — 开源 Windows 兼容操作系统
- [Fluent Design System](https://fluent2.microsoft.design/) — 微软 Fluent Design 官方文档
- [WinUI Gallery](https://github.com/microsoft/WinUI-Gallery) — Fluent 控件参考实现
- Microsoft UX Guidelines for Windows 10
