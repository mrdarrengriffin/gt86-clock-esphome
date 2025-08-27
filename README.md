# GT86 ESPHome Clock 🚗
A custom ESPHome-based digital clock project featuring a Toyota GT86-themed OLED display with RTC backup and WiFi connectivity.

_**Note:** This README is a work in progress and may be updated with more details and instructions. An AI overview was used for now_


## 🌟 Features

- **OLED Display**: 128x32 SSD1306 display with custom pages
- **Real-Time Clock**: DS1307 RTC module for time persistence without WiFi
- **WiFi Connectivity**: Auto-sync time from NTP servers
- **Custom Graphics**: GT86 car silhouette and logo graphics
- **Multiple Display Pages**: Splash screen, time display, and date display
- **Button Control**: Physical button to cycle through display pages
- **Web Interface**: Built-in web server for monitoring
- **Time Persistence**: RTC backup ensures accurate time even during power outages

## 📋 Hardware Requirements

### ESP32 Development Board
- **Model**: ESP32 Mini Kit (or compatible)
- **GPIO Pins Used**:
  - GPIO18/GPIO26: OLED display (SDA/SCL)
  - GPIO17/GPIO16: DS1307 RTC (SDA/SCL)
  - GPIO22: Hour/Page button

### Components
- ESP32 development board (MH-ET LIVE ESP32 MiniKit)
- SSD1306 OLED display (128x32)
- DS1307 Real-Time Clock module
- Push button
- Pull-up resistors (if not built into modules)
- Breadboard and jumper wires

### Wiring Diagram

```
ESP32 Mini Kit    →    Component
GPIO18 (SDA)      →    OLED SDA
GPIO26 (SCL)      →    OLED SCL
GPIO17 (SDA)      →    DS1307 SDA  
GPIO16 (SCL)      →    DS1307 SCL
GPIO22            →    Button (with pull-up)
3.3V              →    VCC (OLED & RTC)
GND               →    GND (OLED & RTC & Button)
```

## 🚀 Quick Start

### 1. Prerequisites

- [ESPHome](https://esphome.io/) installed
- Python 3.8 or higher
- WiFi network credentials

### 2. Configuration

1. **Clone/Download** this repository
2. **Copy secrets file**:
   ```bash
   cp secrets.example.yaml secrets.yaml
   ```
3. **Edit secrets.yaml** with your WiFi credentials:
   ```yaml
   wifi_ssid: "Your_WiFi_SSID"
   wifi_password: "Your_WiFi_Password"
   ap_password: "Your_AP_Password"
   ```

### 3. Installation

1. **Compile and upload**:
   ```bash
   esphome run clock.yaml
   ```

2. **Monitor logs**:
   ```bash
   esphome logs clock.yaml
   ```

## 🎮 Usage

### Display Pages

The clock cycles through three pages using the button:

1. **Splash Screen**: GT86 car silhouette
2. **Time Display**: 
   - Large digital time (HH:MM format)
   - Blinking colon every second
   - Small logo in corner
3. **Date Display**: Current date (DD/MM/YY format)

### Button Controls

- **Press Button**: Cycle to next display page
- **Hold Button**: (Future feature - time setting)

### Web Interface

Once connected to WiFi, access the web interface at:
```
http://gt86.local
```
or use the IP address shown in the logs.

## ⚙️ Configuration

### Timezone Setting

Edit the timezone in `clock.yaml`:
```yaml
substitutions:
  timezone: "Europe/London"  # Change to your timezone
```

### GPIO Pin Mapping

Modify pin assignments in the substitutions section:
```yaml
substitutions:
  button_gpio:
    hour: GPIO22    # Button pin
  display_gpio:
    sda: GPIO18     # OLED SDA
    scl: GPIO26     # OLED SCL
  rtc_gpio:
    sda: GPIO17     # RTC SDA
    scl: GPIO16     # RTC SCL
```

### Display Customization

- **Font**: Uses included `blockblueprint.medium.ttf`
- **Images**: Custom XBM format graphics in `/images/`
- **Update Rate**: 0.01s for smooth animations

## 📁 Project Structure

```
├── clock.yaml                 # Main ESPHome configuration
├── secrets.yaml              # WiFi credentials (git-ignored)
├── secrets.example.yaml      # Template for secrets
├── blockblueprint.medium.ttf  # Custom font file
├── images/
│   ├── car.xbm               # GT86 car silhouette (128x40)
│   └── logo.xbm              # Logo graphic (32x32)
├── .gitignore                # Git ignore rules
├── .python-version           # Python version specification
└── README.md                 # This file
```

## 🔧 Troubleshooting

### Common Issues

**WiFi Connection Problems**:
- Verify credentials in `secrets.yaml`
- Check WiFi signal strength
- Try fallback AP mode (SSID: "GT86")

**Display Not Working**:
- Check I2C wiring (SDA/SCL pins)
- Verify 3.3V power supply
- Test I2C address (0x3C for SSD1306)

**Time Not Syncing**:
- Ensure WiFi connection is stable
- Check timezone configuration
- Verify DS1307 RTC wiring and battery

**Button Not Responding**:
- Check GPIO22 connection
- Verify pull-up resistor is present
- Test button continuity

### Debug Mode

Enable verbose logging by adding to `clock.yaml`:
```yaml
logger:
  level: DEBUG
```

## 🎨 Customization

### Adding Custom Graphics

1. Create XBM format images
2. Place in `/images/` directory
3. Add to configuration:
   ```yaml
   image:
     - id: my_image
       file: "images/my_image.xbm"
       type: BINARY
   ```

### Modifying Display Pages

Edit the `lambda` functions in the display section to customize the layout and content.

### Font Changes

Replace `blockblueprint.medium.ttf` with your preferred font and update the font section accordingly.

## 📝 License

This project is open source. Feel free to modify and distribute according to your needs.

## 🤝 Contributing

Contributions are welcome! Please feel free to submit issues, feature requests, or pull requests.

## 📚 Resources

- [ESPHome Documentation](https://esphome.io/)
- [ESP32 Pinout Reference](https://randomnerdtutorials.com/esp32-pinout-reference-gpios/)
- [SSD1306 OLED Guide](https://randomnerdtutorials.com/esp32-ssd1306-oled-display-arduino-ide/)
- [DS1307 RTC Module](https://lastminuteengineers.com/ds1307-arduino-tutorial/)

---

Made with ❤️ for Toyota GT86 enthusiasts
