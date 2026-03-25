# 🌡️ DHT11 Temperature & Humidity Sensor — Raspberry Pi

Read real-time temperature and humidity from a **DHT11 sensor** using a **Raspberry Pi** and Python. Supports terminal output, optional **CSV logging**, and configurable read intervals.

![Raspberry Pi](https://img.shields.io/badge/Platform-Raspberry%20Pi-C51A4A?logo=raspberry-pi&logoColor=white)
![Python](https://img.shields.io/badge/Language-Python%203-3776AB?logo=python&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-green)

---

## 📷 Circuit Diagram

```
DHT11 Sensor         Raspberry Pi GPIO
───────────────────────────────────────
  VCC  ──────────→  3.3V   (Pin 1)
  GND  ──────────→  GND    (Pin 6)
  DATA ──────────→  GPIO4  (Pin 7)
                    (10kΩ pull-up resistor between DATA and 3.3V)
```

> ⚠️ Use **3.3V**, not 5V — Raspberry Pi GPIO pins are 3.3V logic.

---

## 🛒 Hardware Requirements

| Component               | Qty |
|-------------------------|-----|
| Raspberry Pi (any model)| 1   |
| DHT11 Sensor Module     | 1   |
| 10kΩ Resistor           | 1   |
| Breadboard              | 1   |
| Jumper Wires (F-F)      | ~4  |

---

## ⚙️ Installation

### Option A — Quick Setup Script (Recommended)

```bash
git clone https://github.com/your-username/rpi-dht11-sensor.git
cd rpi-dht11-sensor
chmod +x scripts/setup.sh
./scripts/setup.sh
```

### Option B — Manual

```bash
# System dependency
sudo apt install libgpiod2

# Python dependencies
pip install -r requirements.txt
```

---

## 🚀 Usage

```bash
# Activate virtual environment (if using Option A)
source venv/bin/activate

# Basic run — print to terminal
python src/dht11_sensor.py

# Log readings to CSV
python src/dht11_sensor.py --log

# Custom CSV path and interval
python src/dht11_sensor.py --log --csv my_data.csv --interval 5

# Take exactly 10 readings then stop
python src/dht11_sensor.py --count 10
```

### All CLI Options

| Flag         | Default         | Description                          |
|--------------|-----------------|--------------------------------------|
| `--log`      | off             | Enable CSV logging                   |
| `--csv`      | `sensor_log.csv`| Output CSV file path                 |
| `--interval` | `2.0`           | Seconds between readings             |
| `--count`    | `0` (forever)   | Number of readings (0 = run forever) |

---

## 📊 Sample Output

```
======================================
  DHT11 Sensor — Raspberry Pi
  Press Ctrl+C to stop
======================================
──────────────────────────────────────
  🕐  2025-06-10 14:32:01
  🌡️  Temperature : 27.5 °C  /  81.5 °F
  💧  Humidity    : 58.0 %
  🔥  Heat Index  : 28.3 °C
──────────────────────────────────────
```

### CSV Log Format

```
timestamp,temperature_c,temperature_f,humidity_pct,heat_index_c
2025-06-10 14:32:01,27.5,81.5,58.0,28.3
2025-06-10 14:32:03,27.4,81.32,58.0,28.2
```

---

## 📁 Project Structure

```
rpi-dht11-sensor/
├── src/
│   └── dht11_sensor.py     # Main Python script
├── scripts/
│   └── setup.sh            # One-command dependency installer
├── docs/
│   └── wiring_diagram.md   # Detailed wiring notes
├── images/                 # Photos / screenshots
├── requirements.txt        # Python dependencies
├── LICENSE
└── README.md
```

---

## 🔁 Run on Boot (Systemd Service)

To auto-start the sensor logger on boot:

```bash
sudo nano /etc/systemd/system/dht11.service
```

Paste:

```ini
[Unit]
Description=DHT11 Sensor Logger
After=multi-user.target

[Service]
ExecStart=/home/pi/rpi-dht11-sensor/venv/bin/python /home/pi/rpi-dht11-sensor/src/dht11_sensor.py --log
WorkingDirectory=/home/pi/rpi-dht11-sensor
Restart=always
User=pi

[Install]
WantedBy=multi-user.target
```

```bash
sudo systemctl enable dht11
sudo systemctl start dht11
sudo systemctl status dht11
```

---

## 🐛 Troubleshooting

| Problem                          | Solution                                                     |
|----------------------------------|--------------------------------------------------------------|
| `ModuleNotFoundError`            | Run `pip install -r requirements.txt` inside virtualenv      |
| `RuntimeError: DHT sensor error` | Occasional — script auto-retries; check wiring if persistent |
| No readings                      | Ensure `libgpiod2` is installed (`sudo apt install libgpiod2`)|
| Wrong GPIO pin                   | Edit `GPIO_PIN = board.D4` in `src/dht11_sensor.py`          |
| Permission denied on GPIO        | Run with `sudo` or add user to `gpio` group                  |

---

## 📜 License

MIT License — see [LICENSE](LICENSE) for details.

---

## 🤝 Contributing

Pull requests are welcome!

1. Fork the repo
2. Create your branch (`git checkout -b feature/your-feature`)
3. Commit your changes (`git commit -m 'Add feature'`)
4. Push and open a Pull Request
