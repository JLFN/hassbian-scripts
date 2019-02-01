#Server Side

#I’m running raspbian on a RPi. I use a dedicated user named pi which is added to group dialout to allow access to the device (or you can use root).
## Device can also be connect to with an  3.3v usb TTL or direct to Raspberry Pi TX,RX,3V3,GND. Do not connect to 5V it will destory the  cc2530
cc2530 = P02 -> Raspberry Pi = TX

cc2530 = P03 -> Raspberry Pi = RX

cc2530 = GND -> Raspberry Pi = GND

cc2530 = VCC -> Rasberry  Pi = 3.3V

# Manual test it
```bash
sudo socat tcp-l:1775,reuseaddr,keepalive,nodelay file:/dev/serial/by-id/usb-Silicon_Labs_CP2102_USB_to_UART_Bridge_Controller_0001-if00-port0,nonblock,raw
```
#Server Side
```bash
[Unit]
Description=zigbee-socatvusb
After=network-online.target

[Service]
Type=simple
User=pi
ExecStart=/usr/bin/socat tcp-l:1775,reuseaddr,keepalive,nodelay file:/dev/serial/by-id/usb-Silicon_Labs_CP2102_USB_to_UART_Bridge_Controller_0001-if00-port0,nonblock,raw
Restart=always
StartLimitIntervalSec=0
StartLimitBurst=0

[Install]
WantedBy=multi-user.target
```
# On zigbee2mqtt server side
# Create a systemctl configuration file for socat-vusb
# I’m also running it pi because its on the group "dialout" otherwise you will have a permission issue on the device.
```bash
sudo nano /etc/systemd/system/socat-vusb.service
```
# Add the following to this file:
```bash
[Unit]
Description=socat-vusb
After=network-online.target

[Service]
User=pi
ExecStart=/usr/bin/socat /dev/zigbee,b115200,rawer,echo=0 tcp:192.168.1.175:1775,reuseaddr,su=nobody
Restart=always
RestartSec=10

[Install]
WantedBy=multi-user.target
```
# Save the file and exit.
Verify that the configuration works:
```bash
sudo systemctl start socat-vusb
```
# Update systemd
```bash
sudo systemctl --system daemon-reload
 ```
# Verify that the configuration works:
```bash
sudo systemctl start socat-vusb.service
```
# Show status
```bash
systemctl status socat-vusb.service
```
# Now that everything works, we want systemctl to start socat-vusb automatically on boot, this can be done by executing:
```bash
sudo systemctl enable socat-vusb.service
```
# Some tips that can be handy later:

# Stopping socat-vusb
```bash
sudo systemctl stop socat-vusb
```
# Starting socat-vusb
```bash
sudo systemctl start socat-vusb
```
# View the log of socat-vusb
```bash
sudo journalctl -u socat-vusb.service -f
```


# And the relevant my zigbee2mqtt config
```bash
sudo nano \opt\zigbee2mqtt\data\configuration.yaml
```
```yaml
serial:
  port: /dev/zigbee
advanced:
  rtscts: false
```
# source 
https://community.home-assistant.io/t/rpi-as-z-wave-zigbee-over-ip-server-for-hass/23006/45
https://hub.docker.com/r/forepe/zigbee2mqtt-socat
https://github.com/Koenkk/zigbee2mqtt/issues/442
https://github.com/Koenkk/zigbee2mqtt/issues/665#issuecomment-445722057
