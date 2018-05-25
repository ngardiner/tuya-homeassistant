# tuya-homeassistant

This is a beta fork of the tuya-homeassistant HASS component, with switch and bulb support merged in, and with recent switch development merged into the component.

This is a simple platform to control **SOME** switch devices that use the Tuya cloud for control. Tuya devices are low-cost automation devices for power socket switching and LED lighting based on the ESP chipset.

It uses the pytuya library (https://github.com/clach04/python-tuya) to directly control Tuya protocol devices, which are designed to be controlled from the Tuya cloud. If TCP port 6668 is open on the device (switch/light), it should be controllable by this component. There is no need to manually install the python-tuya library as Home Assistant automatically installs it as a dependancy of this component.

See here for how to find localKey and devId: http://seandev.org/tuyainst

## Finding localKey and devId

localKey can be found by sniffing the communications between the switch or bulb and the Tuya HTTP servers. To do this, you will need to have access to a device through which this traffic will pass. 

### Using ngrep

ngrep is the most convenient tool for this

ngrep -d any "localKey" port 80

### Using tcpdump

There is no easy way to grep the contents of tcpdump captures, however they can be viewed and searched within Wireshark.

tcpdump -s 0 -w output.pcap

### Using Wireshark

* Apply the filter tcp.port == 80 to capture all HTTP traffic. If you know the IP address of the bulb, add && ip.src == xx.xx.xx.xx to further filter the traffic.
* Look for the localKey value in the body of the HTTP communications.

## Installing HomeAssistant Components

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
