### Repository Notice  

🛠️ This is a fork of **[f4fhh/spyserver_container](https://github.com/f4fhh/spyserver_container)**  

⚠️ **Disclaimer: this fork is not actively maintained** 

Key modifications:  
- Added multi-arch support for linux/arm64/v8 platform  
- Specifically optimized for Raspberry Pi 5  
- Pull Requests submitted to the original project

Special thanks to [@f4fhh](https://github.com/f4fhh) for the original Docker containerization approach for Airspy. 

### Docker Compose Example  

If you want to use this Docker image on RPI, here's a sample docker-compose configuration (adapt to your specific receiver and configuration):  

```yaml  
services:  
  spyserver:  
    image: ghcr.io/smkrv/spyserver_container:latest  
    container_name: spyserver  
    restart: unless-stopped  
    ports:  
      - "5555:5555"  
    privileged: true  
    devices:  
      - "/dev/bus/usb:/dev/bus/usb"  
    environment:  
      - SPYSERVER_BIND_ADDRESS=0.0.0.0  
      - SPYSERVER_LISTENING_PORT=5555  
      - SPYSERVER_DEVICE_TYPE=2  
      - SPYSERVER_GAIN=20  
      - SPYSERVER_PPM=0  
      - SPYSERVER_BIAS_TEE=0  
      - SPYSERVER_CORRECTION=0  
      - SPYSERVER_INITIAL_FREQUENCY=7100000  
      - SPYSERVER_INITIAL_SAMPLERATE=2048000
```
--- 

# spyserver_container
a linux SDR server for RTL-SDR and Airspy devices

Airspy R0, R2, Mini, Airspy HF+ and RTL-SDR can be used as a high performance SDR receiver capable of streaming separate chunks of the spectrum to multiple clients over the LAN or the Internet.

## Main features
- [SPY Server – SDR Server for Linux](https://airspy.com/download/) is a RTL-SDR and Airspy devices SDR server


## Starting the container
- Interactive access
```shell
docker run --rm -p 5555:5555 --privileged -v /dev/bus/usb:/dev/bus/usb -it spyserver_container
```
