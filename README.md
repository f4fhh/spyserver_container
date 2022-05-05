# spyserver_container
a linux SDR server for RTL-SDR and Airspy devices

## Main features
- [SPY Server – SDR Server for Linux](https://airspy.com/download/) is a RTL-SDR and Airspy devices SDR server


## Starting the container
- Interactive access
```shell
docker run --rm -p 5555:5555 --privileged -v /dev/bus/usb:/dev/bus/usb -it spyserver_container
```
