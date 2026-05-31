# Hard Disk Health Inspector

A powerful, cross-platform command-line tool for monitoring and inspecting the health of your hard drives and SSDs. Hard Disk Health Inspector provides real-time S.M.A.R.T. data analysis, temperature monitoring, and predictive failure alerts to help you safeguard your data.

## Features

- **S.M.A.R.T. Data Retrieval** – Read and display detailed S.M.A.R.T. attributes from all connected drives.
- **Health Assessment** – Automatically evaluate drive health based on critical attributes (reallocated sectors, pending sectors, etc.).
- **Temperature Monitoring** – Track drive temperature with configurable warning and critical thresholds.
- **Predictive Failure Alerts** – Get notified when a drive shows signs of imminent failure.
- **Cross-Platform Support** – Works on Linux, macOS, and Windows.
- **Multiple Output Formats** – Supports plain text, JSON, and CSV output for integration with other tools.
- **Scheduled Checks** – Run periodic health checks via cron or Task Scheduler integration.
- **Minimal Dependencies** – Lightweight and easy to install.

## Installation

### Prerequisites

- Python 3.6 or higher
- `pip` package manager

### Install from PyPI

```bash
pip install hard-disk-health-inspector
```

### Install from Source

```bash
git clone https://github.com/abhishekaero/Hard-Disk-Health-Inspector.git
cd Hard-Disk-Health-Inspector
pip install .
```

### Platform-Specific Notes

- **Linux**: Requires `smartmontools` (`smartctl`). Install via your package manager:
  ```bash
  sudo apt-get install smartmontools   # Debian/Ubuntu
  sudo yum install smartmontools       # RHEL/CentOS
  ```
- **macOS**: Install `smartmontools` via Homebrew:
  ```bash
  brew install smartmontools
  ```
- **Windows**: Download and install [smartmontools](https://www.smartmontools.org/) for Windows. Ensure `smartctl.exe` is in your PATH.

## Quick Start

### Basic Usage

Check the health of all drives:

```bash
hd-health-inspector
```

Check a specific drive (Linux example):

```bash
hd-health-inspector --device /dev/sda
```

### Output Examples

**Plain text output:**

```
Device: /dev/sda
Model: Samsung SSD 860 EVO 500GB
Temperature: 35°C
Health Status: GOOD

S.M.A.R.T. Attributes:
  Reallocated_Sector_Ct: 0 (PASS)
  Current_Pending_Sector: 0 (PASS)
  Power_On_Hours: 12345
  ...
```

**JSON output:**

```bash
hd-health-inspector --format json
```

**CSV output:**

```bash
hd-health-inspector --format csv --output report.csv
```

### Scheduled Checks

Run a health check every hour and log results:

```bash
# Linux (cron)
0 * * * * /usr/local/bin/hd-health-inspector --format json >> /var/log/disk-health.log

# Windows (Task Scheduler)
schtasks /create /tn "Disk Health Check" /tr "hd-health-inspector --format json >> C:\logs\disk-health.log" /sc hourly
```

## Configuration

Hard Disk Health Inspector can be configured via command-line arguments or a configuration file.

### Command-Line Options

| Option | Description | Default |
|--------|-------------|---------|
| `--device, -d` | Specific device to check (e.g., `/dev/sda`) | All devices |
| `--format, -f` | Output format: `text`, `json`, `csv` | `text` |
| `--output, -o` | Output file path | stdout |
| `--temp-warn` | Temperature warning threshold (°C) | 50 |
| `--temp-critical` | Temperature critical threshold (°C) | 60 |
| `--verbose, -v` | Enable verbose output | False |
| `--quiet, -q` | Suppress non-critical output | False |

### Configuration File

Create a file `~/.hd-health-inspector.conf` (Linux/macOS) or `%USERPROFILE%\.hd-health-inspector.conf` (Windows):

```ini
[general]
format = json
temp_warn = 45
temp_critical = 55

[alerts]
email_enabled = true
smtp_server = smtp.gmail.com
smtp_port = 587
email_from = you@gmail.com
email_to = admin@example.com
```

## API Documentation

Hard Disk Health Inspector can be used as a Python library in your own scripts.

### Module: `hd_health_inspector`

```python
from hd_health_inspector import DiskInspector

# Initialize inspector
inspector = DiskInspector()

# Get all drives
drives = inspector.list_drives()

# Check a specific drive
result = inspector.check_drive('/dev/sda')

# Access S.M.A.R.T. attributes
print(result.smart_attributes)
print(result.temperature)
print(result.health_status)

# Get JSON representation
json_data = result.to_json()
```

### Class: `DriveHealth`

| Attribute | Type | Description |
|-----------|------|-------------|
| `device` | str | Device path |
| `model` | str | Drive model name |
| `temperature` | float | Current temperature in °C |
| `health_status` | str | `GOOD`, `WARNING`, or `CRITICAL` |
| `smart_attributes` | dict | Dictionary of S.M.A.R.T. attributes |
| `power_on_hours` | int | Total power-on hours |

## Contributing

We welcome contributions! Please follow these guidelines:

1. **Fork the repository** and create a feature branch.
2. **Write tests** for any new functionality.
3. **Ensure code style** follows PEP 8 (use `black` and `flake8`).
4. **Update documentation** as needed.
5. **Submit a pull request** with a clear description of changes.

### Development Setup

```bash
git clone https://github.com/abhishekaero/Hard-Disk-Health-Inspector.git
cd Hard-Disk-Health-Inspector
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install -e ".[dev]"
```

### Running Tests

```bash
pytest tests/
```

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

---

**Disclaimer**: Hard Disk Health Inspector provides health assessments based on S.M.A.R.T. data but does not guarantee data integrity. Always maintain regular backups of important data.