# Solar Monitoring Dashboard

A real-time monitoring dashboard for Victron Smart Solar MPPT 75/15 charge controllers. Displays live solar panel data, battery status, charging metrics, and historical power production charts.

## Features

- **Real-time Monitoring**: Live updates every second showing:
  - Battery voltage, current, and state of charge
  - Solar panel voltage, current, and power output
  - Charge controller state (Bulk/Absorption/Float/Off)
  - Daily and total energy yield

- **Historical Charts**: 24-hour power production graphs with minute-level resolution

- **Responsive Design**: Works on desktop, tablet, and mobile devices

- **Dark Mode**: Toggle between light and dark themes

- **Emulation Mode**: Test the dashboard without hardware using realistic simulated data

## Hardware Requirements

- Victron Smart Solar MPPT 75/15 charge controller
- VE.Direct to USB interface cable
- Raspberry Pi (or any Linux system with USB ports)

## Installation on Raspberry Pi

### 1. Install Dependencies

```bash
# Update system packages
sudo apt update && sudo apt upgrade -y

# Install Node.js 20
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt install -y nodejs

# Install Python 3 and pip (usually pre-installed on Raspberry Pi OS)
sudo apt install -y python3 python3-pip

# Install pyserial
pip3 install pyserial
```

### 2. Clone and Setup

```bash
# Clone this repository
git clone <your-repo-url>
cd solar-monitor

# Install Node.js dependencies
npm install
```

### 3. Connect VE.Direct Interface

- Connect the VE.Direct to USB cable between your MPPT controller and the Raspberry Pi
- The device should appear as `/dev/ttyUSB0` (verify with `ls /dev/ttyUSB*`)
- If it appears as a different device, edit `server/routes.ts` and update the path in `startVEDirectReader()`

### 4. Run the Application

```bash
# Development mode
npm run dev

# Production mode (after building)
npm run build
npm start
```

The dashboard will be available at `http://localhost:5000` or `http://<raspberry-pi-ip>:5000`

## Auto-Start on Boot

To run the dashboard automatically when your Raspberry Pi boots:

### 1. Create a systemd service

```bash
sudo nano /etc/systemd/system/solar-monitor.service
```

### 2. Add the following content (adjust paths as needed):

```ini
[Unit]
Description=Solar Monitoring Dashboard
After=network.target

[Service]
Type=simple
User=pi
WorkingDirectory=/home/pi/solar-monitor
ExecStart=/usr/bin/npm start
Restart=always
RestartSec=10

[Install]
WantedBy=multi-user.target
```

### 3. Enable and start the service

```bash
sudo systemctl daemon-reload
sudo systemctl enable solar-monitor
sudo systemctl start solar-monitor

# Check status
sudo systemctl status solar-monitor
```

## Testing Without Hardware

The application includes an emulation mode that automatically activates if no VE.Direct device is detected. This generates realistic solar data based on time of day:

- Solar power follows a daily sine curve (active 6 AM - 6 PM)
- Battery voltage adjusts based on charging state
- All metrics update in real-time

Simply run the application and it will detect the missing hardware and switch to emulation mode automatically.

## Architecture

### Backend
- **Express.js** server handling HTTP and WebSocket connections
- **Python reader** (`vedirect_reader.py`) parsing VE.Direct serial protocol
- **WebSocket** (`/ws` endpoint) for real-time data streaming
- **In-memory storage** with 24-hour rolling window (minute-level aggregation)

### Frontend
- **React** with TypeScript
- **Recharts** for power production visualization
- **shadcn/ui** component library
- **Tailwind CSS** for styling
- **WebSocket client** with automatic reconnection

## Data Flow

```
MPPT Controller → VE.Direct USB → Python Reader → Express Server → WebSocket → React Dashboard
                                         ↓
                                   Memory Storage
                                   (24h history)
```

## API Endpoints

- `GET /api/solar/latest` - Get current solar data
- `GET /api/solar/history?hours=24` - Get historical chart data
- `WebSocket /ws` - Real-time data stream

## Troubleshooting

### No data appearing
- Check that the VE.Direct cable is properly connected
- Verify the device appears: `ls -la /dev/ttyUSB*`
- Check serial port permissions: `sudo usermod -a -G dialout $USER` (then reboot)
- View logs: `sudo journalctl -u solar-monitor -f`

### Chart not updating
- Check WebSocket connection in browser console
- Verify the backend is running and receiving data
- Wait at least 60 seconds for the first chart point to appear

### Permission denied errors
- Ensure your user is in the `dialout` group for serial port access
- Check file permissions on the project directory

## Development

```bash
# Install dependencies
npm install

# Run in development mode (with hot reload)
npm run dev

# Build for production
npm run build

# Start production server
npm start
```

## VE.Direct Protocol

The application reads the VE.Direct text protocol which outputs data in tab-separated key-value pairs. Key fields include:

- `V` - Battery voltage (mV)
- `VPV` - Panel voltage (mV)
- `PPV` - Panel power (W)
- `I` - Battery current (mA)
- `CS` - Charge state (0=Off, 3=Bulk, 4=Absorption, 5=Float)
- `H19` - Yield total (0.01 kWh)
- `H20` - Yield today (0.01 kWh)
- `H21` - Maximum power today (W)

For full protocol details, see the [Victron VE.Direct documentation](https://www.victronenergy.com/support-and-downloads/technical-information).

## License

MIT

## Credits

Built using:
- [Victron Energy VE.Direct Protocol](https://www.victronenergy.com)
- [React](https://react.dev)
- [Express](https://expressjs.com)
- [Recharts](https://recharts.org)
- [shadcn/ui](https://ui.shadcn.com)
