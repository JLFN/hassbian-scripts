```
Esp8266 flashing
Press the Flashmode button when connection to the computer

Downlad the lastest https://github.com/letscontrolit/ESPEasy/releases
Extract the ESPMega zip file, and open ‘FlashESP8266.exe’
Select the right COM port and select the right Firmware (ESP_Easy_mega-XXXXXXXX_normal_ESP8266_4096.bin)

When it is ready close the flashing tool
The ESP8266 will now emit a wifi signal, connect with it with the following password: ‘configesp’ (more information at https://www.letscontrolit.com/wiki/index.php/ESPEasy#Introduction)
After connection a screen opens in which you can let the ESP8266 connect to your WIFI, if succesfull its new IP address will be shown.
Go to this IP address in your browser and click on devices
or Go to http://192.168.4.1/setup
Click on "Devices" Edit of the first task and select ‘Communication - Serial Server’ from the dropdown list
Fill in the form as following:
a.    Name: ZIGBEE2MQTT
b.    Enabled: checked
c.    TCP Port: a number between 1000 and 9999 "1775"
d.    Baud Rate: 115200
e.    Data bits: 8
f.    Parity: No Parity
g.    Stop bits: 1
h.    Reset target after boot: - none –
i.    RX receive timeout: 0
j.    Event processing: Generic
Press Submit

Then its complete the device will get devicename ESP-Easy-0 check your router for the IP or you will get directed after the setup of the Wifi the first time you connected to the device.

You can also try to use ser2net on Linux then use that port.
https://www.letscontrolit.com/wiki/index.php/Ser2Net
Mount with socat
```
