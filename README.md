# HVAC Delta-T Monitor

A comprehensive HVAC monitoring solution using ESP8266, temperature sensors, and static pressure monitoring to track system efficiency and health.

## Project Overview

This project monitors key HVAC metrics to help optimize system performance and detect maintenance issues:

- **Temperature Differential (Delta-T)**: Return air vs supply air temperature
- **Static Pressure**: System pressure differential for airflow monitoring
- **Real-time Dashboard**: Home Assistant integration with comprehensive visualizations
- **Automated Alerts**: Notifications for efficiency issues and maintenance needs

##  Hardware Components

### Required Components
- **ESP8266 Development Board** (NodeMCU, Wemos D1 Mini, etc.)
- **Sensirion SDP810-500PA** - Differential pressure sensor (±500 Pa)
- **2x DS18B20** - Waterproof temperature sensors
- **2x XH-3 Pin Connectors** - For temperature sensor connections
- **1x 4.7kΩ Resistor** - OneWire bus pull-up
- **Breadboard/PCB** - For connections
- **Jumper wires** and **pressure tubing**

### Optional Components
- **2x 4.7kΩ Resistors** - I2C pull-ups (usually built into ESP boards)
- **Reset button** - For manual reset during programming

##  Pin Configuration

| Component | Pin | ESP8266 Pin | Function |
|-----------|-----|-------------|----------|
| SDP810-500PA | VDD | 3.3V | Power |
| SDP810-500PA | GND | GND | Ground |
| SDP810-500PA | SDA | D2 (GPIO4) | I2C Data |
| SDP810-500PA | SCL | D1 (GPIO5) | I2C Clock |
| DS18B20 (both) | VCC | 3.3V | Power |
| DS18B20 (both) | GND | GND | Ground |
| DS18B20 (both) | DATA | D6 (GPIO12) | OneWire Bus |

##  Installation

### 1. Hardware Assembly
1. Follow the [schematic diagram](docs/schematic.md) for wiring
2. Install 4.7kΩ pull-up resistor on OneWire bus (GPIO12 to 3.3V)
3. Connect pressure sensor tubes to supply and return plenums
4. Mount temperature sensors in return and supply air streams

### 2. ESPHome Configuration
1. Copy `delta-tango.yaml` to your ESPHome configuration directory
2. Update WiFi credentials and other secrets as needed
3. Flash firmware to ESP8266:
   ```bash
   esphome run delta-tango.yaml
   ```

### 3. Home Assistant Integration
1. Add the device through ESPHome integration
2. Import the dashboard configuration from `home-assistant/dashboard.yaml`
3. Install optional custom cards via HACS:
   - `mushroom-template-card`
   - `mini-graph-card`

##  Dashboard Features

### Current Status
- Real-time temperature readings
- Delta-T gauge with efficiency indicators
- Static pressure monitoring
- System health status

### Historical Analysis
- 24-hour temperature trends
- 7-day statistical analysis
- Delta-T performance tracking
- Pressure trend monitoring

### Smart Alerts
- HVAC efficiency status
- Filter maintenance indicators
- Airflow restriction warnings

##  Automation & Alerts

The system includes automated monitoring for:

### Filter Maintenance
- **Trigger**: Delta-T drops below 7°F or static pressure rises significantly
- **Action**: Notification to check/replace filter

### System Efficiency
- **Excellent**: Delta-T ≥ 14°F
- **Good**: Delta-T 10-14°F
- **Fair**: Delta-T 7-10°F
- **Poor**: Delta-T < 7°F (check required)

### Airflow Restrictions
- **High static pressure** + **normal Delta-T** = Closed vents or duct issues
- **Low Delta-T** + **high static pressure** = Dirty filter

##  File Structure

```
hvac-delta-t-monitor/
├── README.md
├── delta-tango.yaml                    # ESPHome configuration
├── docs/
│   ├── schematic.md                    # Hardware schematic
│   ├── installation.md                # Detailed installation guide
│   └── troubleshooting.md             # Common issues and solutions
├── home-assistant/
│   ├── dashboard.yaml                 # Dashboard configuration
│   └── automations.yaml              # Alert automations
├── hardware/
│   ├── bom.md                         # Bill of materials
│   └── enclosure/                     # 3D printable enclosure files
└── images/
    ├── dashboard-screenshot.png
    ├── hardware-assembly.jpg
    └── installation-photos/
```

##  Configuration

### ESPHome Sensors
The system creates the following entities:
- `sensor.delta_tango_192b39_hvac_return_air_temperature`
- `sensor.delta_tango_192b39_hvac_supply_air_temperature`
- `sensor.delta_tango_192b39_hvac_delta_t_degf`
- `sensor.delta_tango_192b39_hvac_delta_t_degc`
- `sensor.delta_tango_192b39_hvac_static_pressure` (when SDP810 added)

### Customization
- Update sensor names in `delta-tango.yaml` if needed
- Modify dashboard thresholds in `home-assistant/dashboard.yaml`
- Adjust automation triggers in `home-assistant/automations.yaml`

##  Understanding the Data

### Delta-T Interpretation
- **Cooling Mode**: Higher Delta-T = better efficiency (14-20°F optimal)
- **Heating Mode**: Higher Delta-T = better efficiency (30-40°F optimal)
- **Low Delta-T**: May indicate dirty filter, low refrigerant, or airflow issues

### Static Pressure Guidelines
- **Normal residential**: 0.2-1.0 inches water column (50-250 Pa)
- **Rising pressure**: Filter loading or airflow restrictions
- **Very high pressure**: Check for closed vents or blocked ducts

##  Troubleshooting

### Common Issues
1. **Sensors not detected**: Check I2C/OneWire connections and pull-up resistors
2. **Inaccurate readings**: Verify sensor placement and calibration
3. **Dashboard errors**: Ensure correct entity names in configuration
4. **No pressure readings**: Check tube connections and sensor orientation

### Debugging
- Enable debug logging in ESPHome: `logger: level: DEBUG`
- Use ESPHome logs to verify sensor detection
- Check Home Assistant entity states in Developer Tools

##  Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

##  License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

##  Acknowledgments

- [ESPHome](https://esphome.io/) for the excellent IoT framework
- [Home Assistant](https://www.home-assistant.io/) for home automation platform
- [Sensirion](https://www.sensirion.com/) for quality sensor documentation
- HVAC community for efficiency guidelines and best practices

## Support

- **Issues**: [GitHub Issues](https://github.com/yourusername/hvac-delta-t-monitor/issues)
- **Discussions**: [GitHub Discussions](https://github.com/yourusername/hvac-delta-t-monitor/discussions)
- **Documentation**: [Wiki](https://github.com/yourusername/hvac-delta-t-monitor/wiki)

---

** Disclaimer**: This project is for educational and monitoring purposes. Always consult HVAC professionals for system modifications or if you're unsure about any readings or maintenance requirements.
