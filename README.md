<p align="center">
<img src="https://github.com/home-assistant/home-assistant-assets/raw/master/loading-screen.gif" width="200">
</p>

**Home Assistant** is an open-source home automation platform written in Python ([Github](https://github.com/home-assistant/home-assistant)) on the backend, and [Polymer](https://www.polymer-project.org/) on the frontend. When combined with hardware it makes an open hub for managing the state of home automation devices and triggering automations.

[Presented on February 13, 2018 @ Code & Supply, Pittsburgh](https://www.youtube.com/watch?v=jbQftoWDe8o)

[Documentation](https://home-assistant.io/docs/)  
[Installation](https://home-assistant.io/docs/installation/)  
[Components](https://home-assistant.io/components/)  
[Demo](https://home-assistant.io/demo/)

<img src="https://github.com/ArnaudLoos/HomeAssistant-Presentation/raw/master/images/frontend_full.png" width="600">

<img src="https://github.com/ArnaudLoos/HomeAssistant-Presentation/raw/master/images/frontend_mobile.png" width="600">


#### HA Advantages:

* Speed of development - updates generally released bi-weekly
* Number of integrations approaching 1,000
* Runs on Raspberry Pi, Synology NAS, or any computer running python
* Not dependent on cloud services (many components), all data stored locally in sqllite DB
* Ability to use disparate hardware
* Can work in conjunction with hubs from other manufacturers. Add HA in front of Smartthings
* Integrates with Amazon Echo, Google Assistant, and HomeKit for voice
* Helpful community - active [Discord](https://discord.gg/c5DvZ4e) channel and [Forum](https://community.home-assistant.io/)
* [iOS app](https://home-assistant.io/docs/ecosystem/ios/), or easily accessible through a responsive [web interface](https://home-assistant.io/docs/frontend/mobile/)  

<br>

> In many cases, the adoption of the technology [IoT] is being driven 
by businesses eager to gain valuable data from citizens, with 
little concern for their privacy or the protection of that data.  
All of the analysts consulted pointed out that personal data 
from the connected home will often be bought and sold 
with consumers largely remaining oblivious to potential 
implications.  
[Internet of Things: Pinning down the IoT](https://fsecureconsumer.files.wordpress.com/2018/01/f-secure_pinning-down-the-iot_final.pdf)

<br>

>Blink disables support for Smartthings in preparation for sale to Amazon  
[Hacker News](https://news.ycombinator.com/item?id=15989302)

<br>

>Overall, my takeaway is that the smart home is going to create a new stream of information about our daily lives that will be used to further profile and target us. The number of devices alone that are detected chattering away will be used to determine our socioeconomic status. Our homes could become like internet browsers, with unique digital fingerprints, that will be mined for profit just like our daily Web surfing is. If you have a smart home, it’s open house on your data.  
[The House that Spied on Me](https://gizmodo.com/the-house-that-spied-on-me-1822429852)

#### Competitors:

* Samsung Smartthings
* Wink 2
* Apple Homepod
* Belkin Wemo
* Amazon Echo
* Google Home
* OpenSource: OpenHAB, Domoticz

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
In HA, Z-Wave support is provided by python-openzwave  
[Additional hints on optimizing a Z-Wave network](https://drzwave.blog/2017/01/20/seven-habits-of-highly-effective-z-wave-networks-for-consumers/)

#### Zigbee 
Also a mesh network topology  
No guaranteed interoperability between devices of different manufacturers  
Data transmission at max 250 kbit/s   
10m to 20m range  
Operates in 2.4 GHz ISM band  
Generally cheaper than Z-Wave  

#### 433MHz or 315MHz RF 
Good for transmitting one way - doorbells, fans, weather stations  
Sonoff switches are available in RF versions  
[Broadlink RM Pro](http://www.ibroadlink.com/rm/) - controller

#### Wifi devices
Easiest to configure   
Consumes a lot of power relative to other protocols

#### IR devices
Can control anything that accepts a signal from a remote (tv, amplifier, etc)  
[Broadlink RM Mini3](http://www.ibroadlink.com/rmMini3/)

#### Bluetooth
Good up to 10m  


## [Installation](https://home-assistant.io/docs/installation/)

Installation of Home Assistant on a Raspberry Pi is accomplished by flashing the micro SD card with a downloaded image.

***After first boot an installer will download the latest HA build and install in the background.*** 

Home Assistant ***is not*** installed by flashing the SD card. On first boot make sure your device has an active Internet connection otherwise the download and installation of HA will fail. Either plug in an ethernet cable or configure wireless settings before booting. It takes around 15 minutes to complete the installation, and afterwards the Home Assistant interface will be available at ```http://<RasPI_IP>:8123```

### [Hassbian](https://home-assistant.io/docs/installation/hassbian/)
Install HomeAssistant core on a full Debian OS. No add-on packages available natively, but includes a tool called [hassbian-config](https://home-assistant.io/docs/installation/hassbian/customization/) to aid with the installation of some common packages.

Has the benefit and/or drawback of running a full OS. This is beneficial if you want to run additional programs on the machine which are not integrated into the HA ecosystem.

Upgrades are accomplished through a pip3 package update.

```
$ sudo su -s /bin/bash homeassistant
$ source /srv/homeassistant/bin/activate
$ pip3 install --upgrade homeassistant
```


### [Hass.io](https://home-assistant.io/hassio/)
HomeAssistant running in a Docker container on ResinOS  

<img src="https://github.com/ArnaudLoos/HomeAssistant-Presentation/raw/master/images/HA_update.png" width="500">

* Simple built-in updater
* No need to manage underlying OS
* Integrated app store for simple add-on installation and updating
* Ability to integrate [community add-ons](https://github.com/hassio-addons) (web terminal, homebridge, pi-hole) and [third-party repositories](https://home-assistant.io/hassio/installing_third_party_addons/)

## Home Assistant Basics
Home Assistant organizes all the components that comprise your home automation network. This includes information pulled from the Internet (weather forecast, traffic cam image, bitcoin price), the state of a switch or light (on/off), presence detection (home/away), and more.  

For each [component](https://home-assistant.io/components/) that you want to use in Home Assistant, you add an entry to your ```configuration.yaml``` file and specify its settings.

The configuration file is written in [YAML](https://home-assistant.io/docs/configuration/yaml/). YAML is a markup language that utilizes block collections of key:value pairs. It is heavily dependant on indentation and if there is an error in your configuration file it is likely due to incorrect indentation. You can check your config with an [online YAML parser](https://yaml-online-parser.appspot.com/).

It is possible to edit YAML files from within Home Assistant using the [HASS Configuration UI](https://home-assistant.io/docs/ecosystem/hass-configurator/)


```yaml
homeassistant:
  name: Pittsburgh
  unit_system: imperial
  time_zone: America/New_York
  latitude: !secret latitude
  longitude: !secret longitude
  elevation: 330

frontend:
logbook:
  exclude:
    entities:
      - sensor.dark_sky_summary
      - sensor.dark_sky_daily_high_temperature
      - sensor.dark_sky_daily_low_temperature
discovery:
updater:
sun:
map:
config:
history:
recorder:
  purge_interval: 2
  purge_keep_days: 7
logger:
  default: warning
http:
  api_password: my_password

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

Additional sensors can be added by defining the new component in your ```configuration.yaml``` file. The bitcoin price sensor can be added with the following:

 
```yaml
  - platform: bitcoin
    display_options:
      - market_price_usd
```

Adding a switch component like a power plug or light will automatically cause an entity with a toggle switch to be defined in the frontend, allowing you to turn the switch on or off manually.

<img src="https://github.com/ArnaudLoos/HomeAssistant-Presentation/raw/master/images/frontend_lights.png" width="300">

<br>

### The power of home automation however comes from the automations!  


Automations in Home Assistant are defined as trigger, condition, action.  

```
(trigger)    When I arrive home
(condition)  and it is after sunset
(action)     Turn the lights in the living room on
```
With trigger and action being mandatory and condition being optional.

A trigger will look at events happening in the system while a condition only looks at how the system looks right now. A trigger can observe that a switch is being turned on. A condition can only see if a switch is currently on or off.

An example automation to turn on a porch light at sunset.  
The ```id``` attribute is necessary only if you manage automations from within Home Assistant using the [Automation Editor](https://home-assistant.io/docs/automation/editor/), which I don't recommend.  

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
    entity_id: light.front_porch
```
Send me a text message (using the twilio component) if the water detector by my hot water tank registers water.

```
- id: '0002'
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
- id: '0003'
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
      message: 'Movement detected!'
      target:
        - +1412xxxxxxx
```

In the above example I check the state of an ```input_boolean``` named ```enable_security``` and only execute the action if the state is ```ON```

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

I define different ```input_boolean``` and use them as conditionals to ensure my automations only fire exactly when I want them to.

### Scripts and Scenes

If the action you wish your automation to perform is simple, like turning on a light, then you can call the service ```light.turn_on``` directly and pass the ```entity_id```. If, however, you wish to trigger a more complex action then it may be necessary for your action to call ```script.turn_on``` or ```scene.turn_on``` with the ```entity_id``` being the name of the script or scene to execute.

***[Scripts](https://home-assistant.io/docs/scripts/)*** are a sequence of actions that Home Assistant will execute. 

```yaml
script:
  example_script:
    sequence:
      - service: light.turn_on
        entity_id: light.kitchen
      - service: light.turn_on
        entity_id: light.bedroom
      - service: notify.notify
        data:
          message: 'Lights are on!'
```

***[Scenes](https://home-assistant.io/components/scene/)*** capture the states you want certain entities to be.

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

An add-on project named [scenegen](https://home-assistant.io/docs/ecosystem/scenegen/) can be used to create scenes by reading the current state of devices and outputting a corresponding scene.

### Additional Config

***[Splitting up the configuration](https://home-assistant.io/docs/configuration/splitting_configuration/)***

You can move sections of your ```configuration.yaml``` to separate files and  import them like so:

```
group: !include groups.yaml
automation: !include automations.yaml
scene: !include scenes.yaml
input_boolean: !include input_boolean.yaml
script: !include scripts.yaml
```
 
***Grouping devices***  

Devices can be members of multiple groups. Some groups will be used to display devices together on the frontend. Other groups can be hidden (so they're not displayed on the frontend) and called within automations and scripts.   

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
  Downstairs_Lights:
    - light.halllower
    - light.living_room
  Upstairs_Lights:
    - light.bedroom
    - light.hallupper
``` 
***[Customizing](https://home-assistant.io/docs/configuration/customizing-devices/)***

Rename entities, hide entities, add [icons](https://materialdesignicons.com/), and more.   

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


***[Device Tracking](https://home-assistant.io/components/device_tracker/)***

Home Assistant can trigger automations based on your location and show your location on its map.

Location tracking can be done in many different ways, with varying precision:

* iOS App
* iCloud
* Owntracks
* NMap scan
* Many others

Once the first device is discovered HA will create a file named ```known_devices.yaml``` where it will track all the devices it knows about. If you want to rename a device, add a picture, or stop tracking a device, make the change in this file. For each tracked device HA will create a ```device_tracker.device_name``` entity which can be displayed on the frontend.

You can also define zones such that events will trigger when you enter or leave the zone.

Define a zone like so:

```yaml
  - name: Code_&_Supply
    latitude: 40.46001
    longitude: -79.93072
    radius: 150
    icon: mdi:desktop-classic
```

<img src="https://github.com/ArnaudLoos/HomeAssistant-Presentation/raw/master/images/frontend_map.png" width="600">

You even have the option of triggering events based on the direction of travel with the [proximity](https://home-assistant.io/components/proximity/) component. You can have the thermostat raise the temperature as you approach home.

For more precise location detection Owntracks supports the use of iBeacons. You can [turn your Raspberry Pi into an iBeacon](https://andrewmemory.wordpress.com/2016/03/29/turning-a-raspberry-pi-3-into-an-ibeacon/) with a large radius to trigger automations reliably when arriving in your home zone, or place iBeacons in each room of your house to trigger presence based actions at the room level.

## [Add-ons](https://home-assistant.io/addons/)


### SSH, Samba, Git
Add-ons for updating configurations:

* ***Samba*** - Manage Home Assistant files exposed over an SMB share
* ***Git*** - Load and update configuration files for Home Assistant from a Git repository
* ***SSH*** - Enables easy ssh access for controlling the host
  * [hass](https://home-assistant.io/docs/tools/hass/) - command line control over Hass.io (reload, start/stop services, check config)
  * [hassctl](https://github.com/dale3h/hassctl) add-on for controlling hassbian
  * Can also control through GUI

### Duck DNS
Dynamic DNS to access Home Assistant on the Internet.  

***Secure your installation if it is publicly exposed!***

Consider not publicly exposing your Home Assistant installation to the world and instead keeping it secure inside your network. Various access methods such as OpenVPN, [TOR](https://home-assistant.io/docs/ecosystem/tor/), or [ZeroTier](https://www.zerotier.com/) exist which allow for more secure access. Setup instructions for ZeroTier on Raspberry Pi can be found [here](https://iamkelv.in/blog/2017/06/zerotier.html).

### Let's Encrypt
Easily and automatically add a free SSL certificate

### Mosquitto
Enables a local MQTT broker.
 
[MQTT](https://home-assistant.io/components/mqtt/) is a machine-to-machine or “Internet of Things” connectivity protocol on top of TCP/IP. It allows extremely lightweight publish/subscribe messaging transport.  
Network devices can either publish simple information to an MQTT topic or subscribe to a topic to consume the data published there by another device.  
The main advantage of MQTT is that it is a common protocol in the IoT space and allows easy sharing of information across devices that would otherwise be unable to communicate with each other.  

Example: I can build a simple [temperature and humidity sensor](https://home-assistant.io/blog/2015/10/11/measure-temperature-with-esp8266-and-report-to-mqtt/) utilizing cheap microcontrollers like an [ESP8266](https://www.sparkfun.com/products/13678) or a [Wemos D1 mini](https://wiki.wemos.cc/products:d1:d1_mini). These controllers will read the temperature from an attached sensor and have the ability to communicate over Wi-Fi. But how do we  get these values into Home Assistant? MQTT. There are MQTT libraries available for many platforms, including Arduino. You configure your sensor with the IP of your MQTT broker as well as the topic you wish to publish to. Topics and sub-topics can be created on the fly. So for instance, if I place my new sensor in my bedroom I can tell it to publish to the ```home/temp/bedroom/master/``` topic or if I wish to organize it differently I could instead publish to ```/home/bedroom/master/temp/```.  

Then, after I add the MQTT component to my configuration, 

```
mqtt:
  broker: 127.0.0.1
  port: 1883
  client_id: home-assistant-1
  keepalive: 60
  username: !secret mqtt_user
  password: !secret mqtt_pass
```

I define a sensor to read that particular value.

```yaml
  - platform: mqtt 
    name: "Bedroom Temp"
    state_topic: "home/bedroom/master/temp"
    unit_of_measurement: "ºF"
```
And now inside Home Assistant I have a component named `sensor.bedroom_temp` with a value being updated as the MQTT topic gets updated.

***Pro tip:*** Home Assistant just pulls data from the MQTT topics you tell it to. If the information HA is disaplying seems incorrect or inconsistent it could be that the data being sent to Mosquitto is incorrect and not that it's an issue with HA. To troubleshoot these issues it can be beneficial to see the data being fed into Mosquitto from your sensors in real-time. To accomplish this I use the ```mosquitto_sub``` command with verbose output to subscribe to the ```#``` (all) topic. Omit ```--cafile``` if not using certificates.

```mosquitto_sub -t "#" -v --cafile /etc/mosquitto/certs/ca.crt -p 8883 -u <user> -P <password>```

### [TOR](https://home-assistant.io/docs/ecosystem/tor/)
The ability to access your frontend through an onion address on the TOR network. With this enabled, you do not need to open your firewall ports or setup HTTPS to enable secure remote access.



## Examples

Additional security automations.

```yaml
- id: '0005'
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
        message: "Alarm activated!!"
        target:
          - +1412xxxxxxx
    - service: tts.google_say
      entity_id: media_player.chromecast
      data:
        message: "Alarm alarm alarm alarm alarm alarm"
```

[Dasher](https://github.com/maddox/dasher) is a simple way to bridge your Amazon Dash buttons to HTTP services. It is a Node application that listens on the network for Amazon Dash button presses and sends a post command to the Home Assistant REST API. I have one by my bed that I press when I get up in the morning and when I go to bed at night.

***UPDATE:*** Dasher appears to have fallen out of date and no longer correctly runs on a Raspberry Pi. The [Amazon-Dash](https://github.com/nekmo/amazon-dash) project appears to be a working implementation.

<p align="center">
<img src="https://github.com/ArnaudLoos/HomeAssistant-Presentation/raw/master/images/amazondash.jpg" width="200">
</p>

Amazon Dash buttons can be good, cheap, battery powered buttons.  You need to be an Amazon Prime member however to configure it.


Only one of the following automations will kick off depending on the time of day I press the button.

```yaml
- id: '0006'
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

- id: '0007'
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

script.good_morning

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

scene.good_morning

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

ESP8266, Arduino based devices, and many others can communicate via MQTT  

<img src="https://github.com/ArnaudLoos/HomeAssistant-Presentation/raw/master/images/wemosD1.jpg" width="200">

There exists an [HA component](https://home-assistant.io/components/thethingsnetwork/) for [The Things Network](https://www.thethingsnetwork.org/) to tie in to a global IoT network based on LoRaWAN. LoRaWAN hubs cover an area 5 - 15 km to provide wide-area network coverage for low-bandwidth sensors and allow querying of these sensors world-wide. Home Assistant could, for example, read from all the LoRaWAN sensors connected to a private hub monitoring a 30 acre farm.

[Graphing with Grafana and InfluxDB](https://philhawthorne.com/getting-started-with-grafana-influxdb-for-home-assistant/) - display graphs and dashboards of time series data  

And APIs to integrate with everything.

* [Python API](https://dev-docs.home-assistant.io/en/dev/)
* [Websocket API](https://home-assistant.io/developers/websocket_api/)
* [REST API](https://home-assistant.io/developers/rest_api/)
* [Python REST API](https://home-assistant.io/developers/python_api/)


## Advanced Config
***[Templating](https://home-assistant.io/docs/configuration/templating/)*** 

Early on Home Assistant introduced templating which allows variables in scripts and automations. This makes it possible to create conditions or actions based on variables.

```yaml
- id: '0008'
  alias: 'Daily Update'
  hide_entity: true
  trigger:
  - platform: state
    entity_id: input_boolean.daily_update
    from: 'off'
    to: 'on'
  action:
  - service: tts.google_say
    entity_id: media_player.chromecast
    data_template:
      message: >
        The temperature is currently {{states ('sensor.dark_sky_temperature') | round(0) }} degrees and bitcoin is trading at {{states ('sensor.market_price') | round(0) }} dollars
  - service: input_boolean.turn_off
    entity_id: input_boolean.daily_update
```

Templating can be a great way to ensure the robustness and accuracy of your automations. For example, if you rely heavily on presence detection to trigger automations it can be problematic if your phone GPS glitches and places you 200m away from your actual location for a brief period of time. Now your "Away from home" automations kick-off and the lights turn off.  
In this case you can track your presence using multiple sensors (Owntracks, nmap scan, iCloud) and use a template to define the state of a new ```input_boolean```. The template reads the state of all 3 device trackers and only if 2 out of 3 report you as not_home does it set the state of ```input_boolean.User_home``` to ```OFF```. Now use this component in your automations instead of the individual device trackers.

An example of a more complex template:  

```
  - platform: template
    sensors:
      status_smoke_co_alarm:
        value_template: '{%- if is_state("sensor.first_alert_zcombo_smoke_and_carbon_monoxide_detector_alarm_level", "0") %}
                        Idle
                        {%else%}
                          {%- if is_state("sensor.first_alert_zcombo_smoke_and_carbon_monoxide_detector_alarm_type", "1") %}
                          Fire
                          {%- elif is_state("sensor.first_alert_zcombo_smoke_and_carbon_monoxide_detector_alarm_type", "2") %}
                          CO
                          {%- elif is_state("sensor.first_alert_zcombo_smoke_and_carbon_monoxide_detector_alarm_type", "12") %}
                          Testing
                          {%- elif is_state("sensor.first_alert_zcombo_smoke_and_carbon_monoxide_detector_alarm_type", "13") %}
                          Idle
                          {% else %}
                          Unknown
                          {%- endif %}
                        {%endif%}'
        friendly_name: 'Smoke/CO Alarm'
```

***[Themes](https://community.home-assistant.io/c/projects/themes)***  

It is possible to extensively customize Home Assistant's look from within your ```configuration.yaml```

```yaml
  themes:
    dark_theme:
      # Main colors
      primary-color: '#5294E2'                                                        # Header
      accent-color: '#E45E65'                                                         # Accent color
      dark-primary-color: 'var(--accent-color)'                                       # Hyperlinks
      light-primary-color: 'var(--accent-color)'                                      # Horizontal line in about
      # Text colors
      primary-text-color: '#FFFFFF'                                                   # Primary text colour, here is referencing dark-primary-color
      text-primary-color: 'var(--primary-text-color)'                                 # Primary text colour
      secondary-text-color: '#5294E2'                                                 # For secondary titles in more info boxes etc.
      disabled-text-color: '#7F848E'                                                  # Disabled text colour
      label-badge-border-color: 'green'                                               # Label badge border, just a reference value
      # Background colors
      primary-background-color: '#383C45'                                             # Settings background
      secondary-background-color: '#383C45'                                           # Main card UI background
      divider-color: 'rgba(0, 0, 0, .12)'                                             # Divider
      # Table rows
      table-row-background-color: '#353840'                                           # Table row
      table-row-alternative-background-color: '#3E424B'                               # Table row alternative
      # Nav Menu
      paper-listbox-color: '#5294E2'                                                  # Navigation menu selection hoover
      paper-listbox-background-color: '#2E333A'                                       # Navigation menu background
      paper-grey-50: 'var(--primary-text-color)'
      paper-grey-200: '#414A59'                                                       # Navigation menu selection
```

<img src="https://github.com/ArnaudLoos/HomeAssistant-Presentation/raw/master/images/frontend_dark.jpg" width="600">

#### [AppDaemon](https://home-assistant.io/docs/ecosystem/appdaemon/)
AppDaemon is a loosely coupled, multithreaded, sandboxed python execution environment for writing automation apps for Home Assistant. AppDaemon allows for writing more complex automations but is still based on state changes monitored by Home Assistant.

For example I have a script checking my commute time to work in the morning and back home in the afternoon. This is possible because I use [Owntracks](https://home-assistant.io/components/device_tracker.owntracks/) to track my location. Essentially my current coordinates and the coordinates of my destination are passed to Google Maps via an API to determine travel time. This is done continuously in the background by the Google Travel Time component.

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

This script merely checks that component's state value at a specific time, and if it is greater than my defined threshold then it sends me a text.

First I define my apps in ```apps.yaml```  
Defining apps this way allows me to reuse my code to trigger both alerts with different variables.


```
commute_work:
  module: commute
  class: Commute
  time: "8:00:00"
  limit: 30
  sensor: sensor.time_to_work
  tracker: device_tracker.my_iphone

commute_home:
  module: commute
  class: Commute
  time: "16:50:00"
  limit: 30
  sensor: sensor.time_to_home
  tracker: device_tracker.my_iphone
```
And now I write my ```commute.py``` app.

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
        elif currentLocation == 'home':
            message = "Current travel time from home to work is {} minutes".format(commute)
            self.log(message)
            self.call_service("notify/twilio", title="Commute Warning", message = message, target="+1412xxxxxxx")
        else:
            message = "Location unknown so no message sent"
            self.log(message)
```

#### [HA Dashboard](https://home-assistant.io/docs/ecosystem/hadashboard/)
HA Dashboard is a modular, skinnable dashboard for Home Assistant that is intended to be wall mounted, and is optimized for distance viewing. Perfect for displaying on a cheap Android tablet or Kindle Fire.

<img src="https://github.com/ArnaudLoos/HomeAssistant-Presentation/raw/master/images/HA_dashboard.png" width="600">


#### [Floorplan](https://community.home-assistant.io/c/third-party/floorplan)
Floorplan is a custom integration which allows you to show a floorplan of your home and overlay device control over top. It is very customizable.

<img src="https://github.com/ArnaudLoos/HomeAssistant-Presentation/raw/master/images/floorplan1.jpg" width="500">

<img src="https://github.com/ArnaudLoos/HomeAssistant-Presentation/raw/master/images/floorplan2.png" width="500">

<img src="https://github.com/ArnaudLoos/HomeAssistant-Presentation/raw/master/images/floorplan3.jpg" width="500">

iOS app to create a floorpan template - [magicplan](https://itunes.apple.com/us/app/magicplan/id427424432?mt=8)  

#### [Node Red](https://nodered.org/)
Node Red is a visual workflow development tool, allowing the creation of complex workflows to control Home Assistant devices.

<img src="https://github.com/ArnaudLoos/HomeAssistant-Presentation/raw/master/images/node-red.png" width="600">

3-part series on using Node Red to create Home Assistant automations.  
[Part 1](http://diyfuturism.com/index.php/2017/11/26/the-open-source-smart-home-getting-started-with-home-assistant-node-red/), 
[Part 2](http://diyfuturism.com/index.php/2017/12/14/basic-node-red-flows-for-automating-lighting-with-home-assistant/), 
[Part 3](http://diyfuturism.com/index.php/2018/01/18/going-further-with-home-automations-in-node-red/)  

## Recommendations for starting out
Hass.io running on a [Raspberry Pi 3](https://www.amazon.com/gp/product/B01C6EQNNK/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=B01C6EQNNK&linkCode=as2&tag=solo07e-20&linkId=0305fc19669ba6f80b8813bdde12c5df) - $50 with power and case.  

Many people start with automating lights. Many lights are either wifi controlled or come with their own hub with a proprietary zigbee radio inside. You also should decide if you want white-only bulbs or color. I've found I prefer white bulbs that can display a range of white from cool to warm. I find the white color projected from color bulbs to be unpleasant. Color LED strips work well for adding a splash of color and effects.

[Yeelight](https://www.amazon.com/YEELIGHT-YLDP03YL-Dimmable-Equivalent-Assistant/dp/B01LRTWQJ0/) Wi-Fi color bulb - $35

[IKEA Tradfri](http://www.ikea.com/us/en/catalog/categories/departments/lighting/36815/) gateway kit with hub and two white bulbs - $80

[Hue](https://www.amazon.com/gp/product/B07353SKDD/ref=as_li_qf_asin_il_tl?ie=UTF8&tag=solo07e-20&creative=9325&linkCode=as2&creativeASIN=B07353SKDD&linkId=771af38f2eedf74f476064f5b5090963) starter kit with hub and four white bulbs - $140   

Osram Lightify - [Hub](https://www.amazon.com/gp/product/B00R1PB2T0/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=B00R1PB2T0&linkCode=as2&tag=solo07e-20&linkId=988d7444b9635be6c56440b7780a9ed9) $30, [White bulb](https://www.amazon.com/gp/product/B00R3ID2BG/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=B00R3ID2BG&linkCode=as2&tag=solo07e-20&linkId=06e21b6fe1d65bf7fec7633c6799629c) $19  

Adding door/window sensors, motion detectors, fire alarms, water leak detectors, etc. requires investment in either Zigbee or Z-Wave.

A cheap, popular, Zigbee based starter kit is the [Xiaomi Security Kit](https://www.gearbest.com/alarm-systems/pp_659225.html) for $60 which includes a Zigbee hub, two door/window sensors, and a push button. The trade-off is that it's meant for Chinese use only. This means the kit is shipped from China, you must purchase a power adapter, and run through the setup process in Chinese. Having done this myself it's actually pretty simple.
 
<img src="https://github.com/ArnaudLoos/HomeAssistant-Presentation/raw/master/images/xiaomi.jpg" width="200">


[Sonoff switches](https://www.itead.cc/sonoff-wifi-wireless-switch.html)  ($5 - $10) are a good option for automating power on/off. You can splice one to the cable of any lamp or to any extension cord to make it controllable. They are available in either Wi-Fi or RF versions. [Amazon link](https://www.amazon.com/gp/product/B074N22WFT/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=B074N22WFT&linkCode=as2&tag=solo07e-20&linkId=1dca0516faaca5d6e46eb26ea7e87597)

<img src="https://github.com/ArnaudLoos/HomeAssistant-Presentation/raw/master/images/sonoff.jpg" width="200">

Flash the Sonoff with [Tasmota](https://github.com/arendst/Sonoff-Tasmota) or [ESPurna](https://bitbucket.org/xoseperez/espurna) third-party firmware. 
These firmware provide OTA updates and MQTT support.

There is a large ecosystem of Z-Wave enabled devices and sensors, and some sensors may only be available in Z-Wave versions. To proceed with Z-Wave you need a Z-Wave hub and the Aeotec Z-Stick (Z-Wave plus version) is a popular option for the Raspberry Pi.
  
[Aeotec Z-Stick Gen5](https://www.amazon.com/gp/product/B00X0AWA6E/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=B00X0AWA6E&linkCode=as2&tag=solo07e-20&linkId=53feaaf88db404e3044607f1ff6e611a) - $45  
<img src="https://github.com/ArnaudLoos/HomeAssistant-Presentation/raw/master/images/aeotec.jpg" height="200">

<br>
<p align="center" style="font-size: 30pt">
Happy Automating!
</p>