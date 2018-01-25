Home Assistant is an open-source home automation platform written in Python. When combined with hardware it makes an open hub for managing the state of home automation devices and triggering automations.

[Documentation](https://home-assistant.io/docs/)  
[Installation options](https://home-assistant.io/docs/installation/)  
[Components](https://home-assistant.io/components/)  
[Demo](https://home-assistant.io/demo/)

#### HA advantages:

* Speed of development, updates generally released bi-weekly
* Number of integrations approaching 1,000
* Runs on Raspberry Pi, Synology NAS, or any computer running python
* Not dependent on cloud services (most components), all data stored locally in sqllite DB
* Ability to use almost any hardware, [regardless of vendor support](https://news.ycombinator.com/item?id=15989302)
* Can work in conjunction with hubs from other manufacturers
* Helpful community, active Discord channel and forum
* Has an [iOS app](https://home-assistant.io/docs/ecosystem/ios/), or easily accessible through [mobile web interface](https://home-assistant.io/docs/frontend/mobile/)

#### Competitors:

* Samsung Smartthings
* Wink 2
* Apple Homekit
* Belkin Wemo
* Amazon Echo and Google Assistant (hubs?)
* Others: Logitech, Lowe's Iris, Blink, High-end systems, Security companies like ADT and Guardian
* OpenSource: OpenHAB

## Home Automation Protocols

#### Z-Wave
mesh networking with master-slave model  
Range up to 100m, 200m when meshed  
Adding more devices helps with communication  
Aeotec Gen-5 stick  
Supports AES encryption  
Assigned Node IDs  
z-wave plus difference? Requires network key?  
Has interoperability layer to ensure all Z-Wave hardware and software work together  
low-latency transmission  
up to 100 kbit/s  
Nodes can be up to 30m apart, transmissions can hop nodes up to 4 times (adds delay)  
Operates on 908MHz band, no interference from 2.4GHz (ISM) devices  
Up to 232 devices  

### Zigbee 
requires hub - zigbee protocol not as standard  
mesh networking  
low-power, low-bandwidth (max 250 kbit/s)  
indoor - 10 to 20m range  
operates at 3.4 GHz   
cheaper than Z-Wave  

#### 433MHz or 315MHz RF 
good for transmitting one way - doorbells, fans, weather stations  
Sonoff switches

#### Wifi devices
consume a lot of power

#### IR devices
Can control anything that accepts a signal from a remote (tv, amplifier, etc)  
[Broadlink RM mini3](http://www.ibroadlink.com/rmMini3/)

#### Bluetooth
Up to 10m signal generally


## [Installation](https://home-assistant.io/docs/installation/)

### [Hassbian](https://home-assistant.io/docs/installation/hassbian/)
Install HomeAssistant core on a full Debian OS. No add-on packages available natively, packages like DuckDNS and Mosquitto MQTT


### [Hass.io](https://home-assistant.io/hassio/)
HomeAssistant running in a Docker container on ResinOS  

* Simpler built-in updater
<img src="https://github.com/ArnaudLoos/HomeAssistant-Presentation/raw/master/images/HA_update.png" width="500">
* Integrated app store for simple add-on installation
* Ability to integrate [community add-ons](https://github.com/hassio-addons) (web terminal, homebridge, pi-hole) and [third-party repositories](https://home-assistant.io/hassio/installing_third_party_addons/)

## Home Assistant Basics

Arbitrary switches  
Alarm enabled and Vacation Mode


## [Add-ons](https://home-assistant.io/addons/)


### SSH, Samba, Git
Add-ons for updating configurations


* Samba - Manage Home Assistant files exposed over an SMB share
* Git - Load and update configuration files for Home Assistant from a GIT repository
* SSH - Enables easy ssh access for controlling the host
  * [hassio](https://home-assistant.io/docs/tools/hass/)
  * Can also control through GUI

### Let's Encrypt
Easily and automatically add a free SSL certificate

### Mosquitto
Enables a local MQTT broker  
[MQTT](https://home-assistant.io/components/mqtt/) is a machine-to-machine or “Internet of Things” connectivity protocol on top of TCP/IP. It allows extremely lightweight publish/subscribe messaging transport.  
Network devices can either publish simple information to an MQTT topic or subscribe to a topic to consume the data published there by another device.  
The main advantage of MQTT is that it is a common protocol in the IoT space and allows easy sharing of information across devices that would otherwise be unable to communicate with each other.  

Example: I can build a simple [temperature and humidity sensor](https://home-assistant.io/blog/2015/10/11/measure-temperature-with-esp8266-and-report-to-mqtt/) utilizing cheap microcontrollers like an [ESP8266](https://www.sparkfun.com/products/13678) or a [Wemos D1 mini](https://wiki.wemos.cc/products:d1:d1_mini). These controllers will read the temperature from an attached sensor and have the ability to communicate over WiFi. But how do we  get these values into Home Assistant? MQTT to the rescue. There are MQTT libraries available for many platforms. You configure your sensor with the IP of your MQTT broker as well as the topic you wish to publish to. Topics and sub-topics can be created on the fly. So for instance if I place my new sensor in my bedroom I can tell it to publish to the home/temp/bedroom/master/ topic or if I wish to organize it differently I could instead publish to /home/upstairs/bedroom/temp/.  

Then, after I define the MQTT component in my configuration, I define a sensor to read that particular value.

```yaml
  - platform: mqtt 
    name: "Bedroom Temp"
    state_topic: "home/bedroom/temperature"
    unit_of_measurement: "ºF"
```
And now inside Home Assistant I have a component named `sensor.bedroom_temp` with the value being fed in directly from the sensor with MQTT as the middle man.

### [TOR](https://home-assistant.io/docs/ecosystem/tor/)
The ability to access your frontend through an onion address on the TOR network. With this enabled, you do not need to open your firewall ports or setup HTTPS to enable secure remote access.



## Examples
Automation and config examples


## Openness and ability to extend

ESP8266, Arduino based devices

Yaml config vs [GUI](https://home-assistant.io/docs/ecosystem/hass-configurator/)  






## Advanced Config
[Templating](https://home-assistant.io/docs/configuration/templating/)  
Alternate supported Databases  
Grafana and InfluxDB  

[AppDaemon](https://home-assistant.io/docs/ecosystem/appdaemon/)  
commute script

[HA Dashboard](https://home-assistant.io/docs/ecosystem/hadashboard/) with examples

Floorplan integration and kiosk mode  
    Kindle FIRE kiosk app name?  
    iOS app to create floorpan - [magicplan](https://itunes.apple.com/us/app/magicplan/id427424432?mt=8).  
https://community.home-assistant.io/t/share-your-floorplan/21315

Advanced configuration through Node Red
[Part 1](http://diyfuturism.com/index.php/2017/11/26/the-open-source-smart-home-getting-started-with-home-assistant-node-red/)
[Part 2](http://diyfuturism.com/index.php/2017/12/14/basic-node-red-flows-for-automating-lighting-with-home-assistant/)
[Part 3](http://diyfuturism.com/index.php/2018/01/18/going-further-with-home-automations-in-node-red/)

## Recommendations for starting out
Hass.io running on a Raspberry Pi 3 - $50 with power and case  
To proceed with z-wave: 
[Aeotec Z-Stick Gen5](https://www.amazon.com/Aeotec-Z-Stick-Z-Wave-create-gateway/dp/B00X0AWA6E/) - $45


Lights:
    Cheaper: Yeelight or IKEA Tradifi
    More: Hue or Osram  
    
[Xiaomi Security Kit](https://www.gearbest.com/alarm-systems/pp_659225.html) - $60 includes zigbee hub, two door/window sensors, and a push button
<img src="https://github.com/ArnaudLoos/HomeAssistant-Presentation/raw/master/images/xiaomi.jpg" width="300">


Sonoff and [Tatsmota](https://github.com/arendst/Sonoff-Tasmota)  
Sonoff & Tasmota (or ESPurna) firmware (Sonoff available in RF or Wifi version. Tasmota adds MQTT support)
These third-party firmwares also add OTA updates