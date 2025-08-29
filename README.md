# GT86 ESPHome Clock 🚗
A custom ESPHome-based digital clock and CAN bus monitor project featuring a Toyota GT86-themed OLED display with RTC backup, WiFi connectivity, and OBD-II CAN bus integration.

_**Note:** This README is a work in progress and may be updated with more details and instructions. An AI overview was used for now_


## 🌟 Features

- **OLED Display**: 128x32 SSD1306 display with custom pages
- **Real-Time Clock**: DS1307 RTC module for time persistence without WiFi
- **WiFi Connectivity**: Auto-sync time from NTP servers
- **Custom Graphics**: GT86 car silhouette and logo graphics
- **Multiple Display Pages**: Splash screen, time display, and date display
- **Button Control**: Multiple buttons for display control and time setting
- **CAN Bus Integration**: MCP2515 CAN controller for OBD-II data (RPM, oil pressure, etc.)
- **Home Assistant Integration**: API support for smart home integration
- **Web Interface**: Built-in web server for monitoring
- **Time Persistence**: RTC backup ensures accurate time even during power outages

## 📋 Hardware Requirements

### ESP32 Development Board
- **Model**: QuinLED ESP32 ([QuinLED ESP32](https://quinled.info/quinled-esp32/))
- **Alternative**: MH-ET LIVE ESP32 MiniKit (compatible)
- **GPIO Pins Used**:
  - GPIO18/GPIO26: OLED display (SDA/SCL)
  - GPIO17/GPIO16: DS1307 RTC (SDA/SCL)
  - GPIO15: Hour button
  - GPIO23: Minute button
  - GPIO24: Zero button
  - GPIO25/GPIO32/GPIO12/GPIO04: MCP2515 CAN controller (SCK/SI/SO/CS)
  - GPIO27: CAN interrupt pin

### Components
- QuinLED ESP32 development board (or MH-ET LIVE ESP32 MiniKit)
- SSD1306 OLED display (128x32)
- DS1307 Real-Time Clock module
- MCP2515 CAN Bus controller module
- 3x Push buttons (hour, minute, zero)
- OBD-II connector with CAN bus access
- Pull-up resistors (if not built into modules)
- Breadboard and jumper wires

### OBD-II CAN Bus Connection
- **Pin 6 (High)**: CAN High signal line
- **Pin 14 (Low)**: CAN Low signal line
- Connect these to your MCP2515 CAN controller module

### Wiring Diagram

```
QuinLED ESP32     →    Component
GPIO18 (SDA)      →    OLED SDA
GPIO26 (SCL)      →    OLED SCL
GPIO17 (SDA)      →    DS1307 SDA  
GPIO16 (SCL)      →    DS1307 SCL
GPIO15            →    Hour Button (with pull-up)
GPIO23            →    Minute Button (with pull-up)
GPIO24            →    Zero Button (with pull-up)
GPIO25 (SCK)      →    MCP2515 SCK
GPIO32 (SI)       →    MCP2515 SI (MOSI)
GPIO12 (SO)       →    MCP2515 SO (MISO)
GPIO04 (CS)       →    MCP2515 CS
GPIO27            →    MCP2515 INT
3.3V              →    VCC (OLED & RTC & MCP2515)
GND               →    GND (All components)

OBD-II Connector  →    MCP2515 CAN Module
Pin 6 (CAN High)  →    CAN H
Pin 14 (CAN Low)  →    CAN L
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
3. **Edit secrets.yaml** with your credentials:
   ```yaml
   wifi_ssid: "Your_WiFi_SSID"
   wifi_password: "Your_WiFi_Password"
   ap_password: "Your_AP_Password"
   api_encryption_key: "32_character_hex_key_for_home_assistant"
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

The clock cycles through three pages using the hour button:

1. **Splash Screen**: GT86 car silhouette
2. **Time Display**: 
   - Large digital time (HH:MM format)
   - Blinking colon every second
   - Small logo in corner
3. **Date Display**: Current date (DD/MM/YY format)

### Button Controls

- **Hour Button (GPIO15)**: Cycle to next display page
- **Minute Button (GPIO23)**: (Future feature - minute adjustment)
- **Zero Button (GPIO24)**: (Future feature - reset/zero function)

### CAN Bus Monitoring

The device connects to your GT86's OBD-II port to monitor:
- Engine RPM
- Oil pressure
- Vehicle speed
- Engine temperature
- And other diagnostic data

**Note**: CAN bus functionality is work in progress - waiting for GT86 for testing.

### Home Assistant Integration

Connect to Home Assistant using the API:
- **API Port**: 6053
- **Encryption**: Required (set in secrets.yaml)
- **Reboot Timeout**: 30 minutes

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
    hour: GPIO15      # Hour/Page button
    minute: GPIO23    # Minute button
    zero: GPIO24      # Zero button
  display_gpio:
    sda: GPIO18       # OLED SDA
    scl: GPIO26       # OLED SCL
  rtc_gpio:
    sda: GPIO17       # RTC SDA
    scl: GPIO16       # RTC SCL
  can_gpio:
    int: GPIO27       # CAN interrupt
    sck: GPIO25       # CAN SPI clock
    si: GPIO32        # CAN SPI MOSI
    so: GPIO12        # CAN SPI MISO
    cs: GPIO04        # CAN SPI chip select
```

### Display Customization

- **Font**: Uses included `blockblueprint.medium.ttf`
- **Images**: Custom XBM format graphics in `/images/`
- **Update Rate**: 0.1s for smooth animations

### CAN Bus Configuration

- **Platform**: MCP2515 controller
- **Bit Rate**: 500kbps (standard for most vehicles)
- **CAN ID**: 4
- **Frame Monitoring**: Ready for OBD-II data capture

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
- Check GPIO connections (15, 23, 24)
- Verify pull-up resistors are present
- Test button continuity

**CAN Bus Issues**:
- Verify OBD-II connector wiring (pins 6 & 14)
- Check MCP2515 SPI connections
- Ensure proper CAN bus termination
- Verify vehicle is running (some cars require ignition on)

**Home Assistant Connection**:
- Check API encryption key matches
- Verify port 6053 is accessible
- Ensure ESPHome integration is installed

### Debug Mode

Enable verbose logging by adding to `clock.yaml`:
```yaml
logger:
  level: DEBUG
```

**Warning**: Setting logger to DEBUG level may crash the ESP32 due to excessive CAN bus logs. Use INFO level for normal operation.

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

### Adding CAN Bus Data Display

Once CAN bus data is captured, you can display it by adding sensors and modifying the display pages:

```yaml
# Example for future CAN data integration
sensor:
  - platform: template
    name: "Engine RPM"
    id: engine_rpm
    unit_of_measurement: "RPM"
```

### Font Changes

Replace `blockblueprint.medium.ttf` with your preferred font and update the font section accordingly.

## 📝 License

This project is open source. Feel free to modify and distribute according to your needs.

## 🤝 Contributing

Contributions are welcome! Please feel free to submit issues, feature requests, or pull requests.

## 📚 Resources

- [ESPHome Documentation](https://esphome.io/)
- [QuinLED ESP32 Board Info](https://quinled.info/quinled-esp32/)
- [ESP32 Pinout Reference](https://randomnerdtutorials.com/esp32-pinout-reference-gpios/)
- [SSD1306 OLED Guide](https://randomnerdtutorials.com/esp32-ssd1306-oled-display-arduino-ide/)
- [DS1307 RTC Module](https://lastminuteengineers.com/ds1307-arduino-tutorial/)
- [MCP2515 CAN Bus Module](https://randomnerdtutorials.com/esp32-can-bus-mcp2515/)
- [OBD-II Pinout Reference](https://en.wikipedia.org/wiki/OBD-II_PIDs)
- [Home Assistant ESPHome Integration](https://www.home-assistant.io/integrations/esphome/)

---

Made with ❤️ for Toyota GT86 enthusiasts
