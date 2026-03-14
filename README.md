# ESP32 床头显示器 | ESP32 Bedside Display

[中文](#中文) | [English](#english)

---

## 中文

基于 ESP32 + ST7735 SPI 屏幕的智能家居床头显示器，显示时间、天气、室内/室外温湿度。

![ESP32](https://img.shields.io/badge/ESP32-ST7735-blue)
![Home Assistant](https://img.shields.io/badge/Home_Assistant-Integration-blue)
![ESPHome](https://img.shields.io/badge/ESPHome-2024.11+-green)

---

### 🎯 功能特性

- ✅ **1.8" SPI 彩屏** - ST7735 驱动，128x160 分辨率
- ✅ **自动亮度调节** - 光敏传感器 + PWM 背光
- ✅ **时间显示** - Home Assistant 同步
- ✅ **天气图标** - 晴/雨/雪/云自动识别
- ✅ **室内温湿度** - ESPHome 传感器
- ✅ **室外温湿度** - OpenWeatherMap 集成
- ✅ **体感温度** - 实时计算
- ✅ **风速显示** - m/s 单位

### 📦 硬件清单

| 组件 | 型号 | 数量 | 备注 |
|------|------|------|------|
| 主控板 | ESP32 DevKit | 1 | ESP32-WROOM |
| 屏幕 | ST7735 | 1 | 1.8" 128x160 |
| 光敏电阻 | GL5528 | 1 | 自动亮度 |
| 电阻 | 10kΩ | 1 | 分压 |
| 杜邦线 | - | 若干 | 连接用 |

### 🔌 接线图

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

### 🚀 快速开始

```bash
# 安装 ESPHome
pip3 install esphome

# 配置密钥
cp secrets.example.yaml secrets.yaml

# 编译上传
esphome compile bedside-display.yaml
esphome upload bedside-display.yaml
```

### 🏠 Home Assistant 集成

**必需实体:**
```yaml
sensor.openweathermap_weather
sensor.openweathermap_apparent_temperature
sensor.openweathermap_wind_speed
sensor.openweathermap_temperature
sensor.openweathermap_humidity
```

### 📁 文件说明

```
esp-bedside-display/
├── README.md                 # 本文件
├── bedside-display.yaml      # ESPHome 主配置
└── secrets.example.yaml      # 密钥模板
```

---

## English

Smart bedside display based on ESP32 + ST7735 SPI screen, showing time, weather, indoor/outdoor temperature and humidity.

### 🎯 Features

- ✅ **1.8" SPI Color Display** - ST7735 driver, 128x160 resolution
- ✅ **Auto Brightness** - Light sensor + PWM backlight
- ✅ **Time Display** - Home Assistant sync
- ✅ **Weather Icons** - Auto detect sunny/rainy/snowy/cloudy
- ✅ **Indoor Climate** - ESPHome sensors
- ✅ **Outdoor Climate** - OpenWeatherMap integration
- ✅ **Feels Like Temp** - Real-time calculation
- ✅ **Wind Speed** - m/s unit

### 📦 Hardware List

| Component | Model | Qty | Notes |
|-----------|-------|-----|-------|
| Main Board | ESP32 DevKit | 1 | ESP32-WROOM |
| Display | ST7735 | 1 | 1.8" 128x160 |
| Photoresistor | GL5528 | 1 | Auto brightness |
| Resistor | 10kΩ | 1 | Voltage divider |
| Jumper Wires | - | Several | Connection |

### 🔌 Wiring Diagram

| ESP32 | ST7735 | Description |
|-------|--------|-------------|
| GPIO2 | CLK | SPI Clock |
| GPIO15 | MOSI | SPI Data |
| GPIO17 | DC | Data/Command |
| GPIO4 | CS | Chip Select |
| GPIO16 | RST | Reset |
| GPIO32 | BLK | Backlight PWM |
| GPIO34 | A0 | Light Sensor |
| 3.3V | VCC | Power |
| GND | GND | Ground |

### 🚀 Quick Start

```bash
# Install ESPHome
pip3 install esphome

# Configure secrets
cp secrets.example.yaml secrets.yaml

# Compile & Upload
esphome compile bedside-display.yaml
esphome upload bedside-display.yaml
```

### 🏠 Home Assistant Integration

**Required Entities:**
```yaml
sensor.openweathermap_weather
sensor.openweathermap_apparent_temperature
sensor.openweathermap_wind_speed
sensor.openweathermap_temperature
sensor.openweathermap_humidity
```

### 📁 File Structure

```
esp-bedside-display/
├── README.md                 # This file
├── bedside-display.yaml      # ESPHome config
└── secrets.example.yaml      # Secrets template
```

---

## 📄 许可证 | License

MIT License

---

## 🙏 致谢 | Credits

- [ESPHome](https://esphome.io/)
- [Home Assistant](https://www.home-assistant.io/)
- [OpenWeatherMap](https://openweathermap.org/)

---

## 📬 联系方式 | Contact

- GitHub: [@Jane84894](https://github.com/Jane84894)
- Issues: [Issues](https://github.com/Jane84894/esp-bedside-display/issues)

---

**⭐ 如果这个项目对你有帮助，请给个 Star! | If this project helps you, please give a Star!**
