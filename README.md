# FairWindSK Settings - Signal K Plugin & WebApp

A comprehensive Signal K plugin and web application for managing FairWindSK navigation system settings. This replaces the original C++ Qt-based settings UI with a modern, responsive web interface.

## Features

- **Modern Web Interface**: Beautiful, nautical-themed responsive design
- **RESTful API**: Complete REST API for configuration management
- **Real-time Updates**: Live configuration editing and validation
- **Four Configuration Sections**:
  - Main Settings (window configuration, units of measurement)
  - Connection Settings (Signal K server configuration)
  - Signal K Paths (data path mapping)
  - Applications (installed apps management)

## Installation

### Via NPM (Recommended)
```bash
cd ~/.signalk
npm install signalk-fairwindsk-settings
```

### Manual Installation
1. Clone or download this repository
2. Copy to your Signal K node modules directory:
```bash
cp -r signalk-fairwindsk-settings ~/.signalk/node_modules/
```

3. Restart Signal K server

## Usage

### Access the WebApp

1. Open Signal K server in your browser (default: http://localhost:3000)
2. Navigate to **Webapps** → **FairWindSK Settings**
3. Configure your settings across the four tabs
4. Click **Save Configuration** to persist changes

### REST API Endpoints

The plugin exposes the following REST API endpoints:

#### Get Configuration
```http
GET /plugins/fairwindsk-settings/config
```
Returns the complete FairWindSK configuration as JSON.

#### Update Configuration (Full Replace)
```http
PUT /plugins/fairwindsk-settings/config
Content-Type: application/json

{
  "main": { ... },
  "connection": { ... },
  "signalk": { ... },
  "apps": [ ... ],
  "units": { ... }
}
```

#### Partial Update Configuration
```http
PATCH /plugins/fairwindsk-settings/config
Content-Type: application/json

{
  "main": {
    "windowWidth": 1920
  }
}
```

#### Reset to Defaults
```http
POST /plugins/fairwindsk-settings/config/reset
```

### Configuration File

The configuration is stored in:
```
~/.signalk/fairwindsk.json
```

## Configuration Structure

```json
{
  "main": {
    "virtualKeyboard": false,
    "windowMode": "centered",
    "windowWidth": 1024,
    "windowHeight": 600,
    "windowTop": 20,
    "windowLeft": 0
  },
  "connection": {
    "server": "http://localhost:3000"
  },
  "signalk": {
    "btw": "navigation.course.calcValues.bearingTrue",
    "cog": "navigation.courseOverGroundTrue",
    ...
  },
  "apps": [
    {
      "name": "app-name",
      "description": "App description",
      "fairwind": {
        "active": true,
        "order": 100
      },
      "signalk": {
        "displayName": "Display Name",
        "appIcon": "path/to/icon.png"
      }
    }
  ],
  "units": {
    "airPressure": "hPa",
    "airTemperature": "C",
    "depth": "mt",
    ...
  }
}
```

## Window Modes

- **Windowed**: Custom position and size
- **Centered**: Centered with custom size
- **Maximized**: Full screen without decorations
- **Full Screen**: Full screen mode

## Supported Units

### Temperature
- Celsius (C)
- Fahrenheit (F)
- Kelvin (K)

### Pressure
- HectoPascal (hPa)
- Millibar (mb)
- PSI (psi)
- mmHg (mmHg)

### Speed
- Knots (kn)
- km/h (kmh)
- mph (mph)
- m/s (ms)

### Distance
- Nautical Miles (nm)
- Kilometers (km)
- Miles (ml)
- Meters (m)

### Depth
- Meters (mt)
- Feet (ft)
- Fathoms (ftm)

## Development

### Project Structure
```
signalk-fairwindsk-settings/
├── index.js              # Plugin backend
├── package.json          # Package configuration
├── public/               # WebApp frontend
│   ├── index.html       # Main HTML
│   ├── styles.css       # Styles
│   ├── app.js           # Application logic
│   └── icon.png         # App icon
└── README.md            # This file
```

### API Integration

Example using fetch:

```javascript
// Get current configuration
const config = await fetch('/plugins/fairwindsk-settings/config')
  .then(r => r.json());

// Update configuration
await fetch('/plugins/fairwindsk-settings/config', {
  method: 'PUT',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify(config)
});

// Reset to defaults
await fetch('/plugins/fairwindsk-settings/config/reset', {
  method: 'POST'
});
```

## Migration from C++ UI

This plugin provides a drop-in replacement for the C++ Qt-based settings interface. The configuration format is compatible, so existing `fairwindsk.json` files will work without modification.

### Key Differences
- Web-based interface accessible from any device
- RESTful API for programmatic access
- Modern, responsive design
- No desktop application required
- Real-time validation and feedback

## Troubleshooting

### Configuration Not Saving
- Check Signal K server logs for errors
- Ensure proper write permissions on `~/.signalk/fairwindsk.json`
- Verify JSON format is valid

### WebApp Not Loading
- Restart Signal K server
- Clear browser cache
- Check browser console for errors

### API Not Responding
- Ensure plugin is enabled in Signal K server settings
- Check Signal K server is running
- Verify correct URL and port

## License

MIT

## Contributing

Contributions are welcome! Please submit pull requests or open issues on the project repository.

## Support

For issues and questions:
- GitHub Issues: [Project Repository]
- Signal K Slack: #fairwindsk channel

## Credits

- Original FairWindSK by the FairWindSK Team
- Signal K by the Signal K project
- Nautical-themed UI design inspired by maritime navigation systems
