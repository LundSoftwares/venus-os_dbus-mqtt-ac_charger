# dbus-mqtt-ac_charger - Emulates a physical AC Charger from info in MQTT data

**First off, a big thanks to [mr-manuel](https://github.com/mr-manuel) that created a bunch of templates that made this possible**

GitHub repository: [LundSoftwares/venus-os_dbus-mqtt-ac_charger](https://github.com/LundSoftwares/venus-os_dbus-mqtt-ac_charger)

### Disclaimer
I'm not responsible for the usage of this script. Use on own risk! 


### Purpose
The script emulates a physical AC Charger in Venus OS. It gets the MQTT data from a subscribed topic and publishes the information on the dbus as the service com.victronenergy.charger.mqtt_ac_charger with the VRM instance 31.


### Config
Copy or rename the config.sample.ini to config.ini in the dbus-mqtt-ac-charger folder and change it as you need it.


#### JSON structure
<details>
<summary>Minimum required</summary> 
  
```ruby
{
"ac_charger": {
    "ac_current":0,
        "DC0":{
            "current":0
        }
    }
}
```
</details>

<details>
<summary>Minimum required with DC0</summary> 
  
```ruby
{
"ac_charger": {
    "ac_current":0,
        "DC0":{
            "current":0,
            "voltage":0,
            "temperature":0
        }
    }
}
```
</details>

<details>
<summary>Minimum required with DC0 and DC1</summary> 
  
```ruby
{
"ac_charger": {
    "ac_current":0,
        "DC0":{
            "current":0,
            "voltage":0,
            "temperature":0
        },
        "DC1": {
            "current": 0,
            "voltage": 0,
            "temperature": 0
        }
    }
}
```
</details>

<details>
<summary>Minimum required with DC0, DC1 and DC2</summary> 
  
```ruby
{
"ac_charger": {
    "ac_current":0,
        "DC0":{
            "current":0,
            "voltage":0,
            "temperature":0
        },
        "DC1": {
            "current": 0,
            "voltage": 0,
            "temperature": 0
        },
        "DC2": {
            "current": 0,
            "voltage": 0,
            "temperature": 0
        }
    }
}
```
</details>

<details>
<summary>Full</summary> 
  
```ruby
{
"ac_charger": {
    "ac_current":0,
    "ac_power":0,
    "ac_currentlimit":0,
    "state":0,
    "mode":0,
    "errorcode":0,
    "relaystate":0,
    "lowvoltagealarm":0,
    "highvoltagealarm":0,
        "DC0":{
            "current":0,
            "voltage":0,
            "temperature":0
        },
        "DC1": {
            "current": 0,
            "voltage": 0,
            "temperature": 0
        },
        "DC2": {
            "current": 0,
            "voltage": 0,
            "temperature": 0
        }
    }
}
```
</details>


### Install
1. Copy the ```dbus-mqtt-ac-charger``` folder to ```/data/etc``` on your Venus OS device

2. Run ```bash /data/etc/dbus-mqtt-ac-charger/install.sh``` as root

The daemon-tools should start this service automatically within seconds.

### Uninstall
Run ```/data/etc/dbus-mqtt-ac-charger/uninstall.sh```

### Restart
Run ```/data/etc/dbus-mqtt-ac-charger/restart.sh```

### Debugging
The logs can be checked with ```tail -n 100 -F /data/log/dbus-mqtt-ac-charger/current | tai64nlocal```

The service status can be checked with svstat: ```svstat /service/dbus-mqtt-ac-charger```

This will output somethink like ```/service/dbus-mqtt-ac-charger: up (pid 5845) 185 seconds```

If the seconds are under 5 then the service crashes and gets restarted all the time. If you do not see anything in the logs you can increase the log level in ```/data/etc/dbus-mqtt-grid/dbus-mqtt-ac-charger.py``` by changing ```level=logging.WARNING``` to ```level=logging.INFO``` or ```level=logging.DEBUG```

If the script stops with the message ```dbus.exceptions.NameExistsException: Bus name already exists: com.victronenergy.grid.mqtt_ac-charger"``` it means that the service is still running or another service is using that bus name.

### Multiple instances
It's possible to have multiple instances, but it's not automated. Follow these steps to achieve this:

1. Save the new name to a variable ```driverclone=dbus-mqtt-ac-charger-2```

2. Copy current folder ```cp -r /data/etc/dbus-mqtt-ac-charger/ /data/etc/$driverclone/```

3. Rename the main ```script mv /data/etc/$driverclone/dbus-mqtt-ac-charger.py /data/etc/$driverclone/$driverclone.py```

4. Fix the script references for service and log
```
sed -i 's:dbus-mqtt-grid:'$driverclone':g' /data/etc/$driverclone/service/run
sed -i 's:dbus-mqtt-grid:'$driverclone':g' /data/etc/$driverclone/service/log/run
```
5. Change the ```device_nam0```e and increase the ```device_instance``` in the ```config.ini```

Now you can install and run the cloned driver. Should you need another instance just increase the number in step 1 and repeat all steps.

### Compatibility
It was tested on Venus OS Large ```v3.01``` on the following devices:

RaspberryPi 3b+

Simulated AC Charger data sent from NodeRed

### Screenshots

<details>
<summary>With DC0</summary> 
  
![image](https://github.com/LundSoftwares/venus-os_dbus-mqtt-ac_charger/assets/23386303/08df5fcb-cdb0-42e7-b69e-6f9685fa8bb8)

</details>

<details>
<summary>With DC0 and DC1</summary> 
  
![image](https://github.com/LundSoftwares/venus-os_dbus-mqtt-ac_charger/assets/23386303/c4456520-15be-48da-8034-d37660c5b1a9)

</details>

<details>
<summary>With DC0, DC1 and DC3</summary> 
  
![image](https://github.com/LundSoftwares/venus-os_dbus-mqtt-ac_charger/assets/23386303/d72840b4-76bf-42a1-9ee6-663c65bdc992)

</details>

<details>
<summary>Full</summary> 
  
![image](https://github.com/LundSoftwares/venus-os_dbus-mqtt-ac_charger/assets/23386303/98f5a1ab-2a9c-459b-b5c7-4a32e0224e3e)
![image](https://github.com/LundSoftwares/venus-os_dbus-mqtt-ac_charger/assets/23386303/8b0b954e-501c-491d-81ca-013d82164842)


</details>


# Sponsor this project

<a href="https://www.paypal.com/donate/?business=MTXQ49TG6YH36&no_recurring=0&item_name=Like+my+work?+%0APlease+buy+me+a+coffee...&currency_code=SEK">
  <img src="https://pics.paypal.com/00/s/MjMyYjAwMjktM2NhMy00NjViLTg3N2ItMDliNjY3MjhiOTJk/file.PNG" alt="Donate with PayPal" />
</a>
