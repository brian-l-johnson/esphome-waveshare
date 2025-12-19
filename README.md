# ESPHome Waveshare E-Ink Weather Dashboard

A weather and calendar dashboard for ESP32 with Waveshare 7.5" e-ink display, designed for Home Assistant integration. This project is based on the Weatherman Dashboard by Madelena Mak.

## Features

- **Weather Display**: Current conditions, temperature, and 3-day forecast
- **Weather Alerts**: Display up to 5 active weather alerts with red highlighting
- **Calendar Integration**: Today's and tomorrow's events from Home Assistant
- **Low Power**: E-ink display only refreshes when data changes
- **Smart Updates**: Automatic refresh on data changes, manual refresh button available
- **Diagnostic Sensors**: WiFi signal strength, display refresh count, and last update timestamp

## Hardware Requirements

- ESP32 development board
- Waveshare 7.5" e-Paper display (model: 7.50in-bV3-bwr) with black, white, and red colors
- Waveshare ESP32 e-Paper driver board (recommended)

### Pinout

The configuration uses the standard Waveshare ESP32 e-Paper board pinout:

- **SPI CLK**: GPIO13
- **SPI MOSI**: GPIO14
- **CS**: GPIO15
- **DC**: GPIO27
- **BUSY**: GPIO25
- **RESET**: GPIO26

## Software Requirements

- [ESPHome](https://esphome.io/)
- [Home Assistant](https://www.home-assistant.io/)
- Custom fonts (included in [fonts](fonts/) directory):
  - Gotham Rounded (Book and Bold)
  - Material Design Icons webfont

## Home Assistant Sensors Required

### Weather Sensors

The dashboard requires the following Home Assistant entities:

**Main Weather Entity**:
- `weather.alison_way` (or customize to your weather entity)

**Forecast Sensors** (create template sensors in Home Assistant):
- `sensor.weather_forecast_today_high`
- `sensor.weather_forecast_today_low`
- `sensor.weather_forecast_today_condition`
- `sensor.weather_forecast_today_precipitation`
- `sensor.weather_forecast_tomorrow_high`
- `sensor.weather_forecast_tomorrow_low`
- `sensor.weather_forecast_tomorrow_condition`
- `sensor.weather_forecast_tomorrow_precipitation`
- `sensor.weather_forecast_tomorrow_name`
- `sensor.weather_forecast_day3_high`
- `sensor.weather_forecast_day3_low`
- `sensor.weather_forecast_day3_condition`
- `sensor.weather_forecast_day3_precipitation`
- `sensor.weather_forecast_day3_name`

**Weather Alerts**:
- `sensor.weatheralerts_1` (alert count)
- `sensor.weatheralerts_1_alert_1_most_recent_active_alert`
- `sensor.weatheralerts_1_alert_2_most_recent_active_alert`
- `sensor.weatheralerts_1_alert_3_most_recent_active_alert`
- `sensor.weatheralerts_1_alert_4_most_recent_active_alert`
- `sensor.weatheralerts_1_alert_5_most_recent_active_alert`

### Calendar Sensors

Create template sensors that provide event lists separated by pipe characters (|):
- `sensor.today_events_list` (e.g., "9:00 Meeting|14:00 Doctor|18:00 Dinner")
- `sensor.tomorrow_events_list`

## Installation

1. **Clone this repository**:
   ```bash
   git clone https://github.com/yourusername/esphome-waveshare.git
   cd esphome-waveshare
   ```

2. **Create a secrets.yaml file** in the root directory:
   ```yaml
   wifi_ssid: "YourWiFiSSID"
   wifi_password: "YourWiFiPassword"
   api_encryption_key: "your-32-character-encryption-key"
   ```

3. **Customize the configuration**:
   - Edit [eink-dailyinfo-landscape.yml](eink-dailyinfo-landscape.yml)
   - Update the weather entity (line 236) to match your Home Assistant weather integration
   - Update sensor entity IDs to match your Home Assistant setup

4. **Install to ESP32**:
   ```bash
   esphome run eink-dailyinfo-landscape.yml
   ```

## Configuration

### Display Layout

The display is in landscape mode (800x480) with a two-panel layout:

- **Left Panel (400px)**: Weather information
  - Current day and date
  - Current weather icon and temperature
  - Weather alerts (if active, shown in red)
  - 3-day forecast (Today, Tomorrow, Day 3)

- **Right Panel (400px)**: Calendar events
  - Today's events (up to 5)
  - Tomorrow's events (up to 3)
  - Shows "No events" when calendar is empty

### Update Behavior

The display automatically updates when:
- New sensor data is received from Home Assistant
- The refresh screen button is pressed in Home Assistant
- Data changes are detected (checked every minute)

### Home Assistant Controls

Three buttons are created in Home Assistant:
- **Weatherman - Shutdown**: Safely shut down the ESP32
- **Weatherman - Restart**: Restart the ESP32
- **Weatherman - Refresh Screen**: Force a display refresh

## Customization

### Fonts

Custom fonts are located in the [fonts](fonts/) directory. You can replace them with your preferred fonts, but make sure to update the font definitions in the YAML configuration.

### Weather Icons

The dashboard uses Material Design Icons for weather conditions. The icon mapping is defined in the display lambda function (starting at line 439 in [eink-dailyinfo-landscape.yml](eink-dailyinfo-landscape.yml:439)).

### Temperature Units

The display is currently set to Fahrenheit (line 584). Change to Celsius by replacing `°F` with `°C` and adjusting the calculation if needed.

## Troubleshooting

### Display shows "LOADING"

The display will show "LOADING" until initial sensor data is received from Home Assistant. This is normal on first boot.

### Display not updating

- Check that Home Assistant is online and the API is connected
- Verify sensor entity IDs match your Home Assistant configuration
- Check the logs in ESPHome for errors
- Try pressing the "Weatherman - Refresh Screen" button in Home Assistant

### WiFi connection issues

- Verify WiFi credentials in `secrets.yaml`
- Check WiFi signal strength (diagnostic sensor available in Home Assistant)
- Ensure ESP32 is within range of your WiFi access point

## Credits

- Original Weatherman Dashboard design by [Madelena Mak](https://mmak.es)
- ESPHome community for e-ink display integration
- Material Design Icons for weather icons

## License

This project is open source. Please check individual font licenses in the [fonts](fonts/) directory.

## Contributing

Contributions are welcome! Please feel free to submit pull requests or open issues for bugs and feature requests.
