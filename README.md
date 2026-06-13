# Better Thermostat

[![活跃安装量](https://badge.t-haber.de/badge/better_thermostat?kill_cache=1)](https://github.com/KartoffelToby/better_thermostat/)
[![GitHub Issues](https://img.shields.io/github/issues/KartoffelToby/better_thermostat?style=for-the-badge)](https://github.com/KartoffelToby/better_thermostat/issues)
[![版本 - 1.8.1](https://img.shields.io/badge/Version-1.8.1-009688?style=for-the-badge)](https://github.com/KartoffelToby/better_thermostat/releases)
[![Discord](https://img.shields.io/discord/925725316540923914.svg?style=for-the-badge)](https://discord.gg/9BUegWTG3K)
[![hacs_badge](https://img.shields.io/badge/HACS-Default-41BDF5.svg?style=for-the-badge)](https://github.com/hacs/integration)

**更多信息请访问：<https://better-thermostat.org/>**

## 系统要求

- 最低 Home Assistant 版本：`2024.12`
  （_最新测试版本：`2026.6.0`_）

### 配套 UI

我们开发了一个配套的 UI 组件，可以比 Home Assistant 默认温控器卡片显示更多信息。可通过 HACS 安装：[better-thermostat-ui-card](https://github.com/KartoffelToby/better-thermostat-ui-card)

- 如有疑问或需要帮助，请创建新的 [讨论](https://github.com/KartoffelToby/better_thermostat/discussions)，或检查是否已有相关解答
- 如有建议、发现 Bug 或想添加新设备/功能，请创建新的 [Issue](https://github.com/KartoffelToby/better_thermostat/issues)
- 如果想为项目贡献代码，请创建新的 [Pull Request](https://github.com/KartoffelToby/better_thermostat/pulls)

### 功能特性

此集成为你的联网散热器温控阀（TRV）带来智能化能力：

- 使用远离散热器的温度传感器测量真实的房间温度
- 让 TRV 完全兼容 Google Home
- 开窗自动关闭供暖（无需通过自动化编程实现）
- 根据天气预报自动开关供暖
- 或通过室外温度传感器实现同样的功能
- 自动进行阀门维护，防止夏季长期不使用时阀门卡死
- 将多个 TRV 合并为一个（例如一个房间有多个散热器）
- 增强默认的 TRV 算法，加入智能化策略以减少能耗
- 动态预设温度学习与持久化（基准/"无预设"模式记住你上次设置的温度并在重启后恢复）
- **高级控制算法**：可选 MPC、PID、TPI、AI 时间基准或简单目标温度匹配以实现精确控制
- **可选预设模式**：在设置过程中可自由选择启用的预设模式

### 高级控制算法

Better Thermostat 现在支持多种高级控制策略以优化供暖效果：

- **MPC（模型预测控制）**：使用房间和散热器的物理模型来预测未来温度变化，并优化阀门开度。
- **PID 控制器**：经典的 比例-积分-微分 控制器，通过学习房间特性来维持稳定温度。它具备自动整定功能（目前处于测试阶段），可自动找到适合你房间的最佳参数（Kp、Ki、Kd）。
- **TPI（时间比例积分）**：一种通过周期性开关（或调节）阀门来控制温度的方法，可减少超调。
- **AI 时间基准**：使用基于简单测量和计算的自定义算法（并非真正的 AI）来计算所需供暖功率，并通过调整 TRV 校准来实现。这比标准 TRV 内部算法效果更好。

这些模式可以在设备的高级配置中选择。

### 预设温度配置

预设温度现在通过专用的 `number` 实体进行完全可配置的管理。

工作方式：

1. 在设置或配置过程中，你可以选择希望对该温控器启用的 **预设模式（Presets）**。
2. 对于每个启用的预设模式（如 Eco、Comfort、Sleep），系统会创建对应的 `number` 实体（例如 `number.better_thermostat_preset_eco`）。
3. 这些实体位于设备的 **Configuration（配置）** 类别中。
4. 你可以使用这些数字滑块直接调整每个预设的温度。
5. 数值会在 Home Assistant 重启后自动持久化保留。
6. 通过 number 实体更改预设温度时，如果该预设当前处于激活状态，温控器会立即更新。

默认初始值：

```text
离家（Away）:     16.0 °C
加速（Boost）:    24.0 °C
舒适（Comfort）:  21.0 °C
节能（Eco）:      19.0 °C
居家（Home）:     20.0 °C
睡眠（Sleep）:    18.0 °C
活动（Activity）: 22.0 °C
```

### 支持的硬件

我们支持所有兼容 Home Assistant 且显示为 climate 实体的温控器。

**_已测试的集成_**

- Zigbee2Mqtt
- Deconz
- Tado
- generic_thermostat

### 安装配置

通过 HACS 安装此集成，或从 [最新发布](https://github.com/KartoffelToby/better_thermostat/releases/latest) 复制文件。

配置详情请参阅 [文档](docs/Configuration/configuration.md) 或访问我们的网站：[better-thermostat.org](https://better-thermostat.org/configuration)

以下是一些实用的 `configuration.yaml` 配置技巧。

#### 示例：窗/门传感器配置

```yaml
group:
  livingroom_windows:
    name: 客厅窗户
    icon: mdi:window-open-variant
    all: false
    entities:
      - binary_sensor.openclose_1
      - binary_sensor.openclose_2
      - binary_sensor.openclose_3
```

#### 将多个 TRV 合并为一个（分组）

无需担心，Better Thermostat 原生支持分组功能。

---

## 参与贡献？

请查看 [CONTRIBUTING.md](CONTRIBUTING.md) 文件。

## ☕ 支持

如果你想支持这个项目，可以 ☕ [**在这里请作者喝杯咖啡**](https://www.buymeacoffee.com/kartoffeltoby)。

[![请我喝杯咖啡](https://img.buymeacoffee.com/button-api/?text=Buy%20me%20a%20coffee&emoji=&slug=kartoffeltoby&button_colour=0ac982&font_colour=000000&font_family=Cookie&outline_colour=000000&coffee_colour=ffffff)](https://www.buymeacoffee.com/kartoffeltoby)
