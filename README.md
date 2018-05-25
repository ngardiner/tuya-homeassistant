# tuya-homeassistant

This is a beta fork of the tuya-homeassistant HASS component, with switch and bulb support merged in, and with recent switch development merged into the component.

This is a simple platform to control **SOME** switch devices that use the Tuya cloud for control. Tuya devices are low-cost automation devices for power socket switching and LED lighting based on the ESP chipset.

It uses the pytuya library (https://github.com/clach04/python-tuya) to directly control Tuya protocol devices, which are designed to be controlled from the Tuya cloud. If TCP port 6668 is open on the device (switch/light), it should be controllable by this component. There is no need to manually install the python-tuya library as Home Assistant automatically installs it as a dependancy of this component.

See here for how to find localKey and devId: http://seandev.org/tuyainst

To use, copy the files to HASS custom component directory as below:
   * cp switch/tuya.py <hass config dir>/custom_components/switch/
   * cp light/tuya.py <hass config dir>/custom_components/light/
   * Add config below to configuration.yaml:

```
light:
  - platform: tuya
    name: //name
    host: //ip of device
    local_key: //localKey
    device_id: //devId
```

```
switch:
  - platform: tuya
    host: xxx.xxx.xxx.xxx
    local_key: xxxxxxxxxxxxxxxx
    device_id: xxxxxxxxxxxxxxxxxxxx
    switches:
      switch1:
        friendly_name: Switch 1
        id: 1
```

Multiple switches on a single device:
```
switch:
  - platform: tuya
    host: xxx.xxx.xxx.xxx
    local_key: xxxxxxxxxxxxxxxx
    device_id: xxxxxxxxxxxxxxxxxxxx
    switches:
      switch1:
        friendly_name: Switch 1
        id: 1
      switch2:
        friendly_name: Switch 2
        id: 2
      switch3:
        friendly_name: Switch 3
        id: 3
      switch4:
        friendly_name: Switch 4
        id: 4
```
