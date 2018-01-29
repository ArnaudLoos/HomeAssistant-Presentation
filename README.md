<img src="https://github.com/ArnaudLoos/HomeAssistant-Presentation/raw/master/images/HA_logo.jpeg" width="200">

Home Assistant is an open-source home automation platform written in Python ([Github](https://github.com/home-assistant/home-assistant)) on the backend, and [Polymer](https://www.polymer-project.org/) on the frontend. When combined with hardware it makes an open hub for managing the state of home automation devices and triggering automations.

[Documentation](https://home-assistant.io/docs/)  
[Installation options](https://home-assistant.io/docs/installation/)  
[Components](https://home-assistant.io/components/)  
[Demo](https://home-assistant.io/demo/)

<img src="https://github.com/ArnaudLoos/HomeAssistant-Presentation/raw/master/images/frontend_mobile.png" width="600">

<img src="https://github.com/ArnaudLoos/HomeAssistant-Presentation/raw/master/images/frontend_full.png" width="600">

#### HA advantages:

* Speed of development, updates generally released bi-weekly
* Number of integrations approaching 1,000
* Runs on Raspberry Pi, Synology NAS, or any computer running python
* Not dependent on cloud services (most components), all data stored locally in sqllite DB
* Ability to use almost any hardware, [regardless of vendor support](https://news.ycombinator.com/item?id=15989302)
* Can work in conjunction with hubs from other manufacturers. Add HA in front of Smartthings
* Integrates with Amazon Echo, Google Assistant, and HomeKit for voice
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

#### [Z-Wave](https://home-assistant.io/docs/z-wave/installation/)
Mesh networking with master-slave model  
Up to 232 devices  
Range up to 100m, 200m when meshed (adding more devices helps with communication) 
Supports AES encryption  
Low-latency transmission, up to 100 kbit/s  
Nodes can be up to 30m apart, transmissions can hop nodes up to 4 times (adds delay)  
Operates on 908MHz band, no interference from 2.4GHz (ISM) devices  
Has interoperability layer to ensure all Z-Wave hardware and software work together  
[Z-Wave Plus](https://inovelli.com/z-wave-home-automation/z-wave-plus/) - Fifth gen Z-Wave, improved battery life(~50%), range(167m instead of 100m), bandwidth(250%), OTA firmware updates. Need primary controller to be Z-Wave plus for full benefits  

#### Zigbee 
Also a mesh network topology
No guaranteed interoperability between devices of different manufacturers  
Data transmission at max 250 kbit/s   
10m to 20m range  
Operates in 2.4 GHz ISM band  
Generally cheaper than Z-Wave  

#### 433MHz or 315MHz RF 
Good for transmitting one way - doorbells, fans, weather stations  
Sonoff switches

#### Wifi devices
Easiest to configure?  
Consumes a lot of power relative to other protocols

#### IR devices
Can control anything that accepts a signal from a remote (tv, amplifier, etc)  
[Broadlink RM mini3](http://www.ibroadlink.com/rmMini3/)

#### Bluetooth
Good up to 10m  


## [Installation](https://home-assistant.io/docs/installation/)

Installation of Home Assistant on a Raspberry Pi is accomplished by flashing the micro SD card with a downloaded image.

After initial boot an installer will download the latest HA build and run in the background. It takes around 15 minutes to complete, and afterwards the Home Assistant interface will be available at ```http://<RasPI IP>:8123```.

### [Hassbian](https://home-assistant.io/docs/installation/hassbian/)
Install HomeAssistant core on a full Debian OS. No add-on packages available natively, packages like DuckDNS and Mosquitto MQTT must be installed manually afterwards.

### [Hass.io](https://home-assistant.io/hassio/)
HomeAssistant running in a Docker container on ResinOS  

<img src="https://github.com/ArnaudLoos/HomeAssistant-Presentation/raw/master/images/HA_update.png" width="500">

* Simpler built-in updater
* Integrated app store for simple add-on installation
* Ability to integrate [community add-ons](https://github.com/hassio-addons) (web terminal, homebridge, pi-hole) and [third-party repositories](https://home-assistant.io/hassio/installing_third_party_addons/)

## Home Assistant Basics
Home Assistant organizes all the components that comprise your home automation network. This includes information pulled from the Internet (weather forecast, traffic cam image, bitcoin price), the state of a switch or light (on/off), presence detection (home/away), and more.  

For each component that you want to use in Home Assistant, you add code in your ```configuration.yaml``` file to specify its settings.

The configuration file is written in YAML. [YAML](https://home-assistant.io/docs/configuration/yaml/) is a markup language that utilizes block collections of key:value pairs. YAML is heavily dependant on indentation and if there is an error in your configuration file it may be due to incorrect indentation.


```yaml
homeassistant:
  name: Burgh
  unit_system: imperial
  time_zone: America/New_York
  latitude: !secret latitude
  longitude: !secret longitude
  elevation: 330

light:
  - platform: osramlightify
    host: 192.168.1.5

sensor:
  - platform: darksky
    api_key: !secret darksky_api
    units: auto
    monitored_conditions:
      - summary
      - temperature
      - temperature_max
      - temperature_min
      - precip_type
      - precip_probability
```

Each of these items is an entity with a state value and can be seen on the States page in [Developer Tools](https://home-assistant.io/docs/tools/dev-tools/) in its raw form  

<img src="https://github.com/ArnaudLoos/HomeAssistant-Presentation/raw/master/images/state_darksky.png" width="500">  

and displayed on the frontend for easy viewing.   

<img src="https://github.com/ArnaudLoos/HomeAssistant-Presentation/raw/master/images/frontend_weather.png" width="300">



Additional sensors can be added by defining the component in your configuration.yaml file. The bitcoin price sensor can be added with the following:

 
```yaml
sensor:
  - platform: bitcoin
    display_options:
      - market_price_usd
```

Adding a switch component like a power plug or light will automatically cause an entity with a toggle switch to be defined in the frontend allowing you to turn the switch on or off manually.

<img src="https://github.com/ArnaudLoos/HomeAssistant-Presentation/raw/master/images/frontend_lights.png" width="300">

### The power of home automation however comes from the automations!  

Automations in Home Assistant are defined as trigger, condition, action.  

```
(trigger)    When I arrive home
(condition)  and it is after sunset:
(action)     Turn the lights in the living room on
```
With trigger and action being mandatory and condition being optional.

A trigger will look at events happening in the system while a condition only looks at how the system looks right now. A trigger can observe that a switch is being turned on. A condition can only see if a switch is currently on or off.

An example automation to turn on a porch light at sunset.  

```yaml
- id: '0001'
  alias: 'Outside light ON at sunset'
  hide_entity: true
  trigger:
  - platform: state
    entity_id: sun.sun
    from: above_horizon
    to: below_horizon
  action:
  - service: light.turn_on
    entity_id: light.front_door
```
Send me a text message (using the twilio component) if the water detector by my hot water tank registers water.

```
- id: '1102'
  alias: 'Notify of water heater leak'
  hide_entity: true
  trigger:
    - platform: state
      entity_id: sensor.everspring_st812_flood_detector_flood
      from: '0'
      to: '255'
  action:
    service: notify.twilio
    data:
      message: 'Water detected by water heater!'
      target:
        - +1412xxxxxxx
```
My security alarm. If my hall motion detector registers motion, and nobody is home, and I have security enabled, then send me a text.

```
- id: '1103'
  alias: 'Security - Motion detected'
  hide_entity: true
  trigger:
    - platform: state
      entity_id: binary_sensor.ecolink_motion_sensor_sensor
      from: 'off'
      to: 'on'
  condition:
    - condition: state
      entity_id: group.all_devices
      state: 'not_home'
    - condition: state
      entity_id: input_boolean.enable_security
      state: 'on'
  action:
    service: notify.twilio
    data:
      message: 'Movement detected at home!'
      target:
        - +14122943661
```

In the above example I check the state of an ```input_boolean``` named ```enable_security``` and only execute the action if it is set to ```ON```

This is a toggle switch on the frontend that I can manually set, or set through a different automation.

<img src="https://github.com/ArnaudLoos/HomeAssistant-Presentation/raw/master/images/frontend_switches.png" width="300">

How do I get this toggle switch? I create it.

```yaml
input_boolean:
  enable_security:
    name: Alarm Enabled
    icon: mdi:shield-outline
    initial: on
```

### Scripts and Scenes

If the action you wish your automation to perform is simple, like turning on a light, then you can call the service ```light.turn_on``` directly and pass the ```entity_id```. If however you wish to trigger a more complex action then it may be necessary for your action to call ```script.turn_on``` or ```scene.turn_on``` with the ```entity_id``` being the name of the script or scene to execute.

[Scripts](https://home-assistant.io/docs/scripts/) are a sequence of actions that Home Assistant will execute. [Scenes](https://home-assistant.io/components/scene/) capture the states you want certain entities to be.

```yaml
script:
  example_script:
    sequence:
      - service: light.turn_on
        entity_id: light.ceiling
      - service: notify.notify
        data:
          message: 'Turned on the ceiling light!'
```

```yaml
scene:
  - name: Romantic
    entities:
      light.tv_back_light: on
      light.ceiling:
        state: on
        xy_color: [0.33, 0.66]
        brightness: 200
  - name: Movies
    entities:
      light.tv_back_light:
        state: on
        brightness: 100
      light.ceiling: off
      media_player.sony_bravia_tv:
        source: HDMI 1
```



[Splitting up the configuration](https://home-assistant.io/docs/configuration/splitting_configuration/)

```
group: !include groups.yaml
automation: !include automations.yaml
scene: !include scenes.yaml
input_boolean: !include input_boolean.yaml
script: !include scripts.yaml
```
 
Grouping devices

```
  Lights:
    - light.front_door
    - light.hallupper
    - light.halllower
    - light.living_room
    - light.bedroom
  Inside_Lights:
    - light.halllower
    - light.hallupper
    - light.living_room
    - light.bedroom
  Downstairs:
    - light.halllower
    - light.living_room
  Upstairs:
    - light.bedroom
    - light.hallupper
``` 
Customizing - rename entities, don't display entities, add [MDI](https://materialdesignicons.com/) icons  

```
sensor.dark_sky_summary:
  friendly_name: Conditions
sensor.dark_sky_temperature:
  friendly_name: Temp
sensor.dark_sky_daily_high_temperature:
  friendly_name: Daily High
sensor.dark_sky_daily_low_temperature:
  friendly_name: Daily Low
sensor.dark_sky_precip:
  friendly_name: Precipitation
sensor.market_price:
  friendly_name: BTC
  icon: mdi:currency-btc
binary_sensor.ecolink_doorwindow_sensor_sensor:
  hidden: true
binary_sensor.ecolink_doorwindow_sensor_sensor_2:
  hidden: true
binary_sensor.ecolink_motion_sensor_sensor:
  hidden: true
```

Map frontend and [Presence detection](https://home-assistant.io/components/#presence-detection)

* iOS App
* Owntracks
* NMap scan
* Integration with car smart entertainment systems
* Others

<img src="https://github.com/ArnaudLoos/HomeAssistant-Presentation/raw/master/images/frontend_map.png" width="600">

Define a zone with a radius like so:

```yaml
  - name: Work
    latitude: 40.450242
    longitude: -79.951144
    radius: 150
    icon: mdi:desktop-mac
```

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

Additional security automations

```yaml
- id: '2076'
  alias: 'Security - Sliding Door opened while in bed'
  hide_entity: true
  trigger:
    - platform: state
      entity_id: sensor.ecolink_binary
      from: 'Closed'
      to: 'Open'
  condition:
    - condition: state
      entity_id: input_boolean.in_bed
      state: 'on'
    - condition: state
      entity_id: input_boolean.enable_security
      state: 'on'
  action:
    service: script.turn_on
    entity_id: script.sound_the_alarm
```

Script to sound the alarm.  

```yaml
sound_the_alarm:
  alias: Alarm Activated
  sequence:
    - service: light.turn_on
      entity_id: group.inside_lights
    - service: notify.twilio
      data:
        message: "Alarm activated!! Someone's in the house!!"
        target:
          - +1412xxxxxxx
    - service: tts.google_say
      entity_id: media_player.chromecast1
      data:
        message: "Alarm  alarm  alarm  alarm  alarm  alarm"
```

[Dasher](https://github.com/maddox/dasher) is a simple way to bridge your Amazon Dash buttons to HTTP services. It is a Node application that listens on the network for Amazon Dash button presses and sends a post command to the Home Assistant REST API. I have one by my bed that I press when I get up in the morning and when I go to bed at night.


Only one of the following automations will kick off depending on the time of day I press the button.

```yaml
- id: '1201'
  alias: 'Bedroom Dash Button - Good Morning'
  hide_entity: true
  trigger:
    - platform: state
      entity_id: input_boolean.dash_bedroom
      from: 'off'
      to: 'on'
  condition:
    - condition: time
      after: '05:00:00'
      before: '12:00:00'
  action:
    service: script.turn_on
    entity_id: script.good_morning

- id: '1202'
  alias: 'Bedroom Dash Button - Good Night'
  hide_entity: True
  trigger:
    - platform: state
      entity_id: input_boolean.dash_bedroom
      from: 'off'
      to: 'on'
  condition:
    - condition: time
      after: '17:00:00'
      before: '04:00:00'
  action:
    service: script.turn_on
    entity_id: script.good_night
```

Script good morning:

```
good_morning:
  alias: "Trigger Morning routine"
  sequence:
    - service: scene.turn_on
      entity_id: scene.good_morning
    - service: input_boolean.turn_off
      entity_id: input_boolean.dash_bedroom
    - service: input_boolean.turn_off
      entity_id: input_boolean.in_bed
```

Scene good morning:

```
- name: Good Morning
  entities:
    light.bedroom:
      state: on
      brightness_pct: 70
      color_temp: 400
      transition: 30
    light.living_room:
      state: on
    light.halllower:
      state: on
```

## Openness and ability to extend

ESP8266, Arduino based devices - communicate via MQTT  

There exists an [HA component](https://home-assistant.io/components/thethingsnetwork/) for [The Things Network](https://www.thethingsnetwork.org/) to tie in to a global IoT network based on LaRaWAN. LaRaWAN hubs cover an area 5 - 15 km to provide wide-area network coverage for low-bandwidth sensors and devices and allow querying of these sensors world-wide.

Home Assistant for agriculture




## Advanced Config
[Templating](https://home-assistant.io/docs/configuration/templating/) - 
Early on Home Assistant introduced templating which allows variables in scripts and automations. This makes it possible to adjust your condition and action based on the information of the trigger.

```yaml
- id: '2001'
  alias: 'Speak temperature'
  hide_entity: true
  trigger:
  - platform: state
    entity_id: input_boolean.speak_temp
    from: 'off'
    to: 'on'
  action:
  - service: tts.google_say
    entity_id: media_player.chromecast1
    data_template:
      message: >
        The temperature is currently {{states ('sensor.dark_sky_temperature') | round(0) }} degrees and bitcoin is trading at {{states ('sensor.market_price') | round(0) }} dollars
  - service: input_boolean.turn_off
    entity_id: input_boolean.speak_temp
```


[Themes](https://community.home-assistant.io/c/projects/themes)  

<img src="https://github.com/ArnaudLoos/HomeAssistant-Presentation/raw/master/images/frontend_dark.jpg" width="600">

#### AppDaemon
[AppDaemon](https://home-assistant.io/docs/ecosystem/appdaemon/) is a loosely coupled, multithreaded, sandboxed python execution environment for writing automation apps for Home Assistant. AppDaemon allows for writing more complex automations but is still based on state changes monitored by Home Assistant.

An example script for checking commute time to work in the morning and back home in the afternoon. This is possible because I use [Owntracks](https://home-assistant.io/components/device_tracker.owntracks/) to track my location. Essentially my current coordinates and the coordinates of my destination are passed to Google Maps via an API to determine travel time. This is done continuously in the background by the Google Travel Time component.

```yaml
  - platform: google_travel_time
    name: Time to Home
    api_key: xxxxxxxxxxxxx
    origin: device_tracker.my_iphone
    destination: zone.home
    options:
      mode: driving
```

<img src="https://github.com/ArnaudLoos/HomeAssistant-Presentation/raw/master/images/frontend_travel.png" width="300">

This script merely checks that component's state value at a specific time, and if it is greater than my defined threshold then it send me a text.


```python
import appdaemon.appapi as appapi

class Commute(appapi.AppDaemon):

  def initialize(self):
    time = self.parse_time(self.args["time"])
    self.run_daily(self.check_travel, time)

  def check_travel(self, kwargs):
    commute = int(self.get_state(self.args["sensor"]))
    currentLocation = str(self.get_state(self.args["tracker"]))
    self.log(commute)
    self.log(currentLocation)
    if commute > int(self.args["limit"]):
        if currentLocation == 'Work':
            message = "Current travel time from work to home is {} minutes".format(commute)
            self.log(message)
            self.call_service("notify/twilio", title="Commute Warning", message = message, target="+1412xxxxxxx")
        elif (currentLocation == 'home') or (currentLocation == 'girlfriend'):
            message = "Current travel time from home to work is {} minutes".format(commute)
            self.log(message)
            self.call_service("notify/twilio", title="Commute Warning", message = message, target="+1412xxxxxxx")
        else:
            message = "Location unknown so no message sent"
            self.log(message)
```

#### HA Dashboard
[HA Dashboard](https://home-assistant.io/docs/ecosystem/hadashboard/) is a modular, skinnable dashboard for Home Assistant that is intended to be wall mounted, and is optimized for distance viewing. Perfect for displaying on a cheap Android tablet or Kindle Fire.

<img src="https://github.com/ArnaudLoos/HomeAssistant-Presentation/raw/master/images/HA_dashboard.png" width="600">


#### Floorplan
[Floorplan](https://community.home-assistant.io/c/third-party/floorplan) is a custom integration which allows you to show a floorplan of your home and overlay device control on top.

<img src="https://github.com/ArnaudLoos/HomeAssistant-Presentation/raw/master/images/floorplan1.jpg" width="500">

<img src="https://github.com/ArnaudLoos/HomeAssistant-Presentation/raw/master/images/floorplan2.png" width="500">

<img src="https://github.com/ArnaudLoos/HomeAssistant-Presentation/raw/master/images/floorplan3.jpg" width="500">

iOS app to create a floorpan template - [magicplan](https://itunes.apple.com/us/app/magicplan/id427424432?mt=8)  

#### Node Red
[Node Red](https://nodered.org/) is a visual workflow development tool, allowing the creation of complex workflows to control Home Assistant devices.

<img src="https://github.com/ArnaudLoos/HomeAssistant-Presentation/raw/master/images/node-red.png" width="600">

3-part series on using Node Red to create Home Assistant automations.  
[Part 1](http://diyfuturism.com/index.php/2017/11/26/the-open-source-smart-home-getting-started-with-home-assistant-node-red/), 
[Part 2](http://diyfuturism.com/index.php/2017/12/14/basic-node-red-flows-for-automating-lighting-with-home-assistant/), 
[Part 3](http://diyfuturism.com/index.php/2018/01/18/going-further-with-home-automations-in-node-red/)  

## Recommendations for starting out
Hass.io running on a Raspberry Pi 3 - $50 with power and case  

Most people start with automating lights. Many lights are either wifi controlled or come with their own hub with a proprietary zigbee radio inside.

[Yeelight](https://www.amazon.com/YEELIGHT-YLDP03YL-Dimmable-Equivalent-Assistant/dp/B01LRTWQJ0/) Wi-Fi color bulb - $35

[IKEA Tradfri](http://www.ikea.com/us/en/catalog/categories/departments/lighting/36815/) gateway kit with hub and two white bulbs - $80

[Hue](https://www.store.meethue.com/) starter kit with hub and two white bulbs - $70



Adding door/window sensors, motion detectors, fire alarms, water leak detectors, etc. requires investment in either Zigbee or Z-Wave

 
    
A cheap, popular Zigbee based starter kit is the [Xiaomi Security Kit](https://www.gearbest.com/alarm-systems/pp_659225.html) for $60 which includes zigbee hub, two door/window sensors, and a push button  
<img src="https://github.com/ArnaudLoos/HomeAssistant-Presentation/raw/master/images/xiaomi.jpg" width="200">


[Sonoff switches](https://www.itead.cc/sonoff-wifi-wireless-switch.html)  ($5) are a good option for automating on/off. You can splice one to the cable of any lamp or to any extension cord to make it controllable. They are available in either Wi-Fi or RF versions.

<img src="https://github.com/ArnaudLoos/HomeAssistant-Presentation/raw/master/images/sonoff.jpg" width="200">

Flash the Sonoff with [Tasmota](https://github.com/arendst/Sonoff-Tasmota) or [ESPurna](https://bitbucket.org/xoseperez/espurna) third-party firmware. 
These firmware provide OTA updates and MQTT support.

There is a large ecosystem of z-wave enabled devices and sensors, and some sensors may only be available in z-wave versions. To proceed with z-wave you need a z-wave hub and the Aeotec Z-Stick is a popular option for the Raspberry Pi.

  
[Aeotec Z-Stick Gen5](https://www.amazon.com/Aeotec-Z-Stick-Z-Wave-create-gateway/dp/B00X0AWA6E/) - $45  
<img src="https://github.com/ArnaudLoos/HomeAssistant-Presentation/raw/master/images/aeotec.jpg" height="200">
