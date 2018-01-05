# anavid
Standalone daemon process for controlling ANAVI Light pHAT over MQTT

# Installation

Ensure that Home Assistant and MQTT broker are installed either on the Raspberry Pi on on another device in the network.

* Install wiringpi

```
sudo apt-get update
sudo apt-get install -y wiringpi
```

* Install Paho (library for MQTT clients)

```
cd ~
git clone https://github.com/eclipse/paho.mqtt.c.git
cd paho.mqtt.c.git
make
sudo make install
```

* Install pigpio

```
git clone https://github.com/joan2937/pigpio.git
cd pigpio
make
sudo make install
```

* Build and install

```
cd anavid
make
sudo make install
```

* Start the system service

```
sudo systemctl start anavi
```

* Retrieve the machine id

```
journalctl -u anavi | grep "Machine ID"
```

For example, the output should be:
```
pi@hassbian:/$ journalctl -u anavi | grep "Machine ID"
Jan 05 01:28:36 hassbian anavid[387]: Machine ID: 26bbc1a1189c44139e080197cbecc2e6
```

* Replace YOURDEVICEID and add the following lines to configuration.yaml

```
# ANAVI Light pHAT
light:
  - platform: mqtt_json
    name: "ANAVI Light pHAT"
    command_topic: "YOURDEVICEID/action/rgbled"
    brightness: true
    rgb: true
```

# MQTT Commands

* Change the colors of the 12V RGB LED strip:

```
mosquitto_pub -h hassbian.local -d -p 1883 -t "replace-with-your-device-id/action/rgbled" -m "{ \"red\":255, \"green\":0, \"blue\":0 }"
```
