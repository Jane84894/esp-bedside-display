# ESP32 床头显示器

基于 ESP32 + ST7735 SPI 屏幕的智能家居床头显示器，显示时间、天气、室内/室外温湿度。

![ESP32](https://img.shields.io/badge/ESP32-ST7735-blue)
![Home Assistant](https://img.shields.io/badge/Home_Assistant-Integration-blue)
![PlatformIO](https://img.shields.io/badge/ESPHome-2024.11+-green)

---

## 📸 效果预览

> **[待补充：实物照片]**

---

## 🎯 功能特性

- ✅ **1.8" SPI 彩屏** - ST7735 驱动，128x160 分辨率
- ✅ **自动亮度调节** - 光敏传感器 + PWM 背光
- ✅ **时间显示** - Home Assistant 同步
- ✅ **天气图标** - 晴/雨/雪/云自动识别
- ✅ **室内温湿度** - ESPHome 传感器
- ✅ **室外温湿度** - OpenWeatherMap 集成
- ✅ **体感温度** - 实时计算
- ✅ **风速显示** - m/s 单位

---

## 📦 硬件清单

| 组件 | 型号 | 数量 | 备注 |
|------|------|------|------|
| 主控板 | ESP32 DevKit | 1 | ESP32-WROOM |
| 屏幕 | ST7735 | 1 | 1.8" 128x160 |
| 光敏电阻 | GL5528 | 1 | 自动亮度 |
| 电阻 | 10kΩ | 1 | 分压 |
| 杜邦线 | - | 若干 | 连接用 |

### 接线图

| ESP32 | ST7735 | 说明 |
|-------|--------|------|
| GPIO2 | CLK | SPI 时钟 |
| GPIO15 | MOSI | SPI 数据 |
| GPIO17 | DC | 数据/命令 |
| GPIO4 | CS | 片选 |
| GPIO16 | RST | 复位 |
| GPIO32 | BLK | 背光 PWM |
| GPIO34 | A0 | 光敏传感器 |
| 3.3V | VCC | 电源 |
| GND | GND | 地 |

---

## 🚀 快速开始

### 1️⃣ 环境准备

```bash
# 安装 ESPHome
pip3 install esphome

# 或使用 Home Assistant 插件
```

### 2️⃣ 配置密钥

```bash
# 复制密钥模板
cp secrets.example.yaml secrets.yaml

# 编辑 secrets.yaml
vim secrets.yaml
```

### 3️⃣ 编译上传

```bash
# 编译
esphome compile bedside-display.yaml

# 上传
esphome upload bedside-display.yaml
```

---

## 🏠 Home Assistant 集成

### 必需实体

```yaml
# 天气
sensor.openweathermap_weather
sensor.openweathermap_apparent_temperature
sensor.openweathermap_wind_speed
sensor.openweathermap_temperature
sensor.openweathermap_humidity

# 室内温湿度 (ESPHome 其他设备)
sensor.esphome_web_living_room_temperature
sensor.esphome_web_living_room_humidity
```

### 可选：农历显示

添加农历传感器后，可以在屏幕上显示：

```yaml
text_sensor:
  - platform: homeassistant
    id: lunar_date
    entity_id: sensor.lunar_date
```

---

## 📁 文件说明

```
esp-bedside-display/
├── README.md                 # 本文件
├── bedside-display.yaml      # ESPHome 主配置
├── secrets.example.yaml      # 密钥模板
└── fonts/                    # 字体文件 (可选)
    ├── number.ttf
    └── misans.ttf
```

---

## ⚙️ 配置说明

### 自动亮度

```yaml
sensor:
  - platform: adc
    pin: GPIO34
    filters:
      - calibrate_linear:
          - 3.18 -> 0.08  # 暗环境
          - 0.14 -> 1.00  # 亮环境
    on_value:
      then:
        - light.turn_on:
            id: screen_backlight
            brightness: !lambda "return x;"
```

### 天气图标逻辑

```cpp
if (weather.find("雷") != std::string::npos) {
  icon = "\U000F0593"; // 雷雨
} else if (weather.find("雨") != std::string::npos) {
  icon = "\U000F0597"; // 雨天
} else if (weather.find("晴") != std::string::npos) {
  // 根据时间判断白天/夜晚
  if (time.hour >= 18 || time.hour < 6) {
    icon = "\U000F0594"; // 夜晚
  } else {
    icon = "\U000F0599"; // 晴天
  }
}
```

---

## 🔧 故障排除

### 屏幕不亮
- 检查 SPI 接线 (CLK/MOSI/CS/DC/RST)
- 确认背光 PWM 配置
- 尝试 `invert_colors: true/false`

### 亮度不自动调节
- 检查光敏电阻接线 (GPIO34)
- 校准 `calibrate_linear` 值
- 查看传感器数值 `esphome logs`

### 天气图标不更新
- 确认 HA 实体 ID 正确
- 检查网络连接
- 查看日志 `weather_state` 变化

---

## 🎨 自定义

### 修改布局
编辑 `display.lambda` 部分：

```cpp
// 修改坐标调整位置
it.printf(6, 12, id(font_icon), ...);  // 图标 X/Y
it.strftime(126, 4, ...);              // 时间 X/Y
```

### 添加农历
```yaml
text_sensor:
  - platform: homeassistant
    id: lunar_date
    entity_id: sensor.lunar_date

# 在 lambda 中添加
it.printf(64, 150, id(font_label), "%s", id(lunar_date).state.c_str());
```

### 更改颜色
```yaml
color:
  - id: color_custom
    red: 100%
    green: 50%
    blue: 0%
```

---

## 📊 功耗数据

| 状态 | 电流 | 备注 |
|------|------|------|
| 屏幕常亮 | ~200mA | 背光 100% |
| 自动亮度 | ~80-150mA | 根据环境光 |
| 待机 | ~50mA | 背光最低 |

---

## 🤝 贡献

欢迎提交 Issue 和 PR!

---

## 📄 许可证

MIT License

---

## 🙏 致谢

- [ESPHome](https://esphome.io/)
- [Home Assistant](https://www.home-assistant.io/)
- [OpenWeatherMap](https://openweathermap.org/)

---

## 📬 联系方式

- GitHub: [@D1ts1337](https://github.com/D1ts1337)
- 问题反馈：[Issues](https://github.com/D1ts1337/esp-bedside-display/issues)

---

**⭐ 如果这个项目对你有帮助，请给个 Star!**
