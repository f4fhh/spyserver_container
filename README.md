# Airspy SpyServer Container

A Docker container for running Airspy SpyServer - a high-performance SDR server supporting RTL-SDR and Airspy devices.

## Overview

This container allows you to run [Airspy SpyServer](https://airspy.com/download/) in a Docker environment, enabling streaming of spectrum data to multiple clients over LAN or Internet. It supports:
- Airspy R0, R2, Mini
- Airspy HF+
- RTL-SDR devices

## Features

- Multi-platform support (amd64, arm64, armv7)
- Easy configuration via environment variables or config file
- Support for Airspy SpyServer features
- USB device passthrough
- Network streaming capabilities

## Quick Start

### Prerequisites

- Docker installed
- RTL-SDR or Airspy device connected
- USB device permissions configured

### Basic Usage

```shell
docker run --rm -p 5555:5555 --privileged -v /dev/bus/usb:/dev/bus/usb -it ghcr.io/f4fhh/spyserver_container:latest
```

### Docker Compose

Create a `docker-compose.yml` (example):

```yaml
services:
  spyserver:
    image: ghcr.io/f4fhh/spyserver_container:latest
    container_name: spyserver
    restart: unless-stopped
    ports:
      - "5555:5555"
    privileged: true
    devices:
      - "/dev/bus/usb:/dev/bus/usb"
    volumes:
      - "./config:/etc/spyserver:ro"  # Optional: for custom configuration
    environment:
      - SPYSERVER_BIND_ADDRESS=0.0.0.0
      - SPYSERVER_LISTENING_PORT=5555
      - SPYSERVER_DEVICE_TYPE=2       # 2 for RTL-SDR
```

Start the container:
```bash
docker compose up -d
```

## Configuration Variables

### Network Settings
| Variable | Description | Default | Notes |
|----------|-------------|---------|-------|
| BIND_PORT | Server listening port | 5555 | TCP port for client connections |
| LIST_IN_DIRECTORY | Enable server listing | 1 | List server in public directory |

### Server Information
| Variable | Description | Default | Notes |
|----------|-------------|---------|-------|
| OWNER_NAME | Server owner name | empty | Public display name |
| OWNER_EMAIL | Contact email | empty | Contact information |
| ANTENNA_TYPE | Type of antenna used | empty | Antenna description |
| ANTENNA_LOCATION | Antenna location | empty | Geographic location |
| GENERAL_DESCRIPTION | Server description | empty | General information |

### Server Limits
| Variable | Description | Default | Notes |
|----------|-------------|---------|-------|
| MAXIMUM_CLIENTS | Maximum concurrent clients | 1 | Connection limit |
| MAXIMUM_SESSION_DURATION | Session time limit (seconds) | 0 | 0 = unlimited |
| ALLOW_CONTROL | Allow client device control | 1 | 1 = enabled, 0 = disabled |

### Device Configuration
| Variable | Description | Default | Notes |
|----------|-------------|---------|-------|
| DEVICE_TYPE | SDR device type | Auto | Auto/RTL-SDR/Airspy |
| DEVICE_SERIAL | Device serial number | 0 | Specific device selection |
| DEVICE_SAMPLE_RATE | Sample rate in Hz | 2500000 | Device sampling rate |
| FORCE_8BIT | Force 8-bit samples | 0 | Commented out by default |
| MAXIMUM_BANDWIDTH | Maximum bandwidth in Hz | 15000 | Commented out by default |

### FFT Settings
| Variable | Description | Default | Notes |
|----------|-------------|---------|-------|
| FFT_FPS | FFT frames per second | 15 | Waterfall update rate |
| FFT_BIN_BITS | FFT bin resolution | 16 | Spectral resolution |

### Frequency Settings
| Variable | Description | Default | Notes |
|----------|-------------|---------|-------|
| INITIAL_FREQUENCY | Starting frequency in Hz | 7100000 | Commented out by default |
| MINIMUM_FREQUENCY | Lower frequency limit | 0 | Commented out by default |
| MAXIMUM_FREQUENCY | Upper frequency limit | 35000000 | Commented out by default |
| FREQUENCY_CORRECTION_PPB | Frequency correction | 0 | Parts per billion |

### Device Tuning
| Variable | Description | Default | Notes |
|----------|-------------|---------|-------|
| INITIAL_GAIN | Initial device gain | 5 | Device-specific gain setting |
| RTL_SAMPLING_MODE | RTL-SDR sampling mode | 0 | Device-specific setting |
| CONVERTER_OFFSET | Frequency converter offset | 0 | For upconverters/downconverters |
| ENABLE_BIAS_TEE | Enable bias tee power | 0 | RTL-SDR bias tee support |

### Buffer Settings
| Variable | Description | Default | Notes |
|----------|-------------|---------|-------|
| BUFFER_SIZE_MS | Buffer size in milliseconds | 50 | Streaming buffer size |
| BUFFER_COUNT | Number of buffers | 10 | Buffer queue length |

All these variables can be set via environment variables in your docker-compose.yml or docker run command or in `config/spyserver.config`. Example:

```yaml
services:
  spyserver:
    environment:
      - BIND_PORT=5555
      - OWNER_NAME=MyStation
      - ANTENNA_TYPE=Dipole
      - DEVICE_SAMPLE_RATE=2048000
      - FFT_FPS=30
      # ... other variables as needed
```

### Custom `spyserver.config` Configuration

Create/mount & and edit `config/spyserver.config`.

## Client Connection

Use SDR# or any compatible client:
1. Install [SDR#](https://airspy.com/download/)
2. Select "AIRSPY Server Network" as source
3. Configure:
   - Server Address: Your server IP
   - Port: 5555 (default)

## Troubleshooting

1. Check device connection:
```bash
lsusb | grep RTL
```

2. View container logs:
```bash
docker compose logs -f
```

3. Common issues:
   - USB device permissions
   - Network connectivity
   - Sample rate compatibility

## License

This project is licensed under the [GNU AGPL v3](LICENSE).

## Acknowledgments

- [Airspy](https://airspy.com/) for the SpyServer
