# Tasmota & Home Assistant

## Environment

- Home Assistant 0.103.6 (python isntallation)
- Mosquitto MQTT 1.5.7
- Tasmota 8.1.0.2 installed in Zeoota Power Strip ZLD-44EU-W, with tuya-convert

## Mosquitto

    sudo apt install -y mosquitto mosquitto-clients
    sudo systemctl enable mosquitto.service

Config users/passwords

    pico users.passwd

Enter one row for each user:password

    user1:ffYf5hOv
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
    


## Home Assistant Configuration

configuration.yaml





