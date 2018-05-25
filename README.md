# tuya-homeassistant

This is a beta fork of the tuya-homeassistant HASS component, with switch and bulb support merged in, and with recent switch development merged into the component.

This is a simple platform to control **SOME** switch devices that use the Tuya cloud for control. Tuya devices are low-cost automation devices for power socket switching and LED lighting based on the ESP chipset.

It uses the pytuya library (https://github.com/clach04/python-tuya) to directly control Tuya protocol devices, which are designed to be controlled from the Tuya cloud. If TCP port 6668 is open on the device (switch/light), it should be controllable by this component. There is no need to manually install the python-tuya library as Home Assistant automatically installs it as a dependancy of this component.

## Finding localKey and devId

localKey can be found by sniffing the communications between the switch or bulb and the Tuya HTTP servers. To do this, you will need to have access to a device through which this traffic will pass. 

I have found that the "Smart Life" app which provides access to manage Tuya devices does not generate a localKey packet every time that a device is removed and re-added.

### Using macOS or Android

   * https://github.com/codetheweb/tuyapi/blob/master/docs/SETUP.md

### Using ngrep

ngrep is the most convenient tool for this capture, if you happen to have it installed on a device which is between the smart switch/bulb and the Internet. The following command will capture relevant packets:

```ngrep -d any "localKey" port 80```

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

## Compatible Devices

### Bulbs

   * https://www.ebay.com.au/itm/APP-WiFi-Smart-LED-Light-Bulb-Dimmable-7W-RGB-E27-B22-Lamp-For-Alexa-Google-Home/132491192747
   * LOHAS: https://www.ebay.com.au/itm/LOHAS-WIFI-A60-Colour-LED-Smart-Bulb-Works-with-Amazon-Alexa-Colour-Changing/182832137163

### Switches

   * https://www.amazon.com/gp/product/B075GWQSYH/ref=oh_aui_detailpage_o01_s00
   * Gosund Smart Plug: https://www.amazon.com/gp/product/B076VMNNRZ/ref=oh_aui_search_detailpage
   * KMC: https://www.amazon.com/gp/product/B07313TH7B/ 
   * Oittm Smart WiFi Wall Switch: https://www.amazon.com/Oittm-Control-Appliances-Function-Smartphone/dp/B073J5R1CT/ref=sr_1_1_sspa
   * QIACHIP WiFi Smart Switch: https://www.aliexpress.com/store/product/QIACHIP-Wifi-Smart-Switch-2-buttons-Wireless-Smart-Home-APP-Touch-Control-Wall-Light-Switch-US/2888005_32845528129.html 
   * VANZAVANZU: https://www.amazon.com/dp/B078PHD6S5/ref=twister_B078PGGJYV?tag=uc_102-20
   * YTE Smart Plug: https://www.amazon.com/gp/product/B077VLLKB8/ref=oh_aui_detailpage_o02_s00
