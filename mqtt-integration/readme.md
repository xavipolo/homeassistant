# Tasmota & Home Assistant

### Scenario

I want to use "Zeoota Power Strip ZLD-44EU-W" and other Tuya based gadgets without using TUYA Servers

I'm using Home Assistant as a Home Automation Server

### Environment

- Home Assistant 0.103.6 (python installation / no hassio)
- Mosquitto MQTT 1.5.7
- Tasmota 8.1.0.2 installed in Zeoota Power Strip ZLD-44EU-W, with tuya-convert


## Mosquitto

We need a MQTT Broker to comunicate TASMOTA devices with Home Assistant.

Installation

    sudo apt install -y mosquitto mosquitto-clients
    sudo systemctl enable mosquitto.service

Config users/passwords

    pico users.passwd

Enter one row for each user:password

    zeoota_user:ffYf5hOv
    user2:kjG3a123

Encrypt file

    mosquitto_passwd -U users.passwd

Modify configuration to use users and passwords

    sudo pico /etc/mosquitto/conf.d/user.conf

add to user.conf file

    allow_anonymous false
    password_file /etc/mosquitto/users.passwd

restart services after any configuration change

    sudo systemctl restart mosquitto.service


Access to logs

    sudo tail -f /var/log/mosquitto/mosquitto.log
    
To Do:
- Add TSL Certificates


## Tasmota

To bypass TUYA Servers, we need a custom firmware in our devices.
There are different methods to overwrite firmware in ESP8266 based devices, some of them requiries a physical connection (soldering wires), but tuya-convert can use a security bug in some tuya versions to allow a wireless upgrading.

Using:
- Raspberry Pi 3
- [Raspian Buster Lite](https://www.raspberrypi.org/downloads/raspbian/)  
- [tuya-convert](https://github.com/ct-Open-Source/tuya-convert)
- Android Phone (Sansung s8+)

Connect to rPi by LAN, because Wifi will be used to enable a hotspot

### Install tuya convert

     git clone https://github.com/ct-Open-Source/tuya-convert
     cd tuya-convert
     ./install_prereq.sh

Start process

     ./start_flash.sh
     
Follow on screen instructions

Installation will make a firmware backup

### Tuya cofiguration

Access with a web browser to IP device (check on your wifi router)

Set Wifi configuration

Set MQTT configuration

Use user/pass created in MQTT configuration

- Host: <your MQTT broker>
- Port: 1883 (by default)    
- User: zeoota_user
- Password: ffYf5hOv
- Topic: zeoota
- Full Topic: homeassistant/switch/%topic%/%prefix%
    
Set Template

Template was submitted to tasmota as [ZEOOTA-ZLD-44E](https://templates.blakadder.com/zeeota_ZLD-33EU-W.html)

![Zeoota Power Strip ZLD-44EU-W Template](https://github.com/xavipolo/homeassistant/blob/master/mqtt-integration/zeoota-template.png)


## Home Assistant Configuration

configuration.yaml





