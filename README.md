# checkmk_fritzbox

For CMK2.2.0 see [AVM FRITZ BOX Smarthome](https://thl-cmk.hopto.org/gitlab/checkmk/various/fritzbox_smarthome)

This repository contains a additional [check_MK](https://mathias-kettner.com/) Fritz!Box Agent which can gather informations over the AVM AHA HTTP Interface about SmartHome Devices connected to an Fritz!Box. In the first version this was a modification this was a modification of the normal checkmk fritzbox agent now it is seperated from it.<br>
It also provides a Check for parsing the output, and WATO-Pages for configuring thresholds.

## CLI Usage
```
./agents/special/agent_fritzbox_smarthome --help
usage: agent_fritzbox_smarthome [-h] [--debug] [--username [USERNAME]]
                                [--password [PASSWORD]] [--port [PORT]]
                                [--protocol [{http,https}]]
                                host

Check_MK Fritz!Box Smarthome Agent

positional arguments:
  host                  Host name or IP address of your Fritz!Box

optional arguments:
  -h, --help            show this help message and exit
  --debug               Debug mode: let Python exceptions come through
  --username [USERNAME]
  --password [PASSWORD]
  --port [PORT]
  --protocol [{http,https}]
```

## Tested Devices
* AVM FRITZ!DECT210 (FW 04.16)
   * Switching Socket
   * Function Values: powermeter, temperature, switch
* AVM FRITZ!DECT200 (FW 04.16)
   * Switching Socket
   * Function Values: powermeter, temperature, switch
* AVM FRITZ!DECT301 (FW: 04.88)
   * Thermostat
   * Function Values: temperature, hkr
* AVM FRITZ!DECTRepeater100 (FW 04.16)
   * DECTRepeater
   * Function Values: temperature
* AVM FRITZ!DECT400 (FW: 04.90)
   * Button
   * Function Values: button
* Eurotronic CometDECT (FW 03.54)
   * Thermostat
   * Function Values: temperature, hkr

## Function Value implementation:
* base (this means generic device infos)
   * Graphs: None
   * Status: When Device present OK, else WARN
* powermeter
   * Graphs: Consumption (kWh), Voltage (V), Power (W)
   * Status: Always okay
* temperature
   * Graphs: Temperature Sensor
   * Status: Always okay
* switch
   * Graphs: Switch State
   * Status: Always okay
* hkr
   * Graphs: Temperature Thermostat, Battery, Window Open
   * Status: Compare against default params
* button
   * Nothing yet (no idea what to check here)

## How to setup
* ensure you are working with checkmk 1.6
* Install current package
   * wget the latest version from https://github.com/MaximilianClemens/checkmk_fritzbox/releases
   * then: mkp install /var/check_mk/packages/fritzbox_smarthome-xyz.mkp
* Configure:
   * Create a new Host for your fritzbox
     * at DATA SOURCES > Check_MK Agent select "No Checkmk agent, all configured agents"
     * Save & Finish
   * Host & Service Parameters 
   * search for fritz
   * Create an new rule in "Check state of Fritz!Box Smarthome Devices"
     * Enter Credentials, Port and Protocol
     * Explicit Host : <your fritzbox host>
     * Save
   * (re)-inventory the (fritzbox) host, the smarthome devices should now be detected

## Credits
* Gerold Gruber <info@edv2g.de> / [edv2g](https://edv2g.de/)
* Maximilian Clemens <maximilian.clemens@mailbox.org> / [gamma.red](https://gamma.red/)

## Informations
[AVM - AHA-HTTP-Interface Dokumentation](https://avm.de/fileadmin/user_upload/Global/Service/Schnittstellen/AHA-HTTP-Interface.pdf)
