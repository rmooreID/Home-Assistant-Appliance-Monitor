# LaundryBot
These days there is no shortage of _smart_ appliances. But does that mean we should neglect our _simple_ pre-IoT machines just because they can't send a push notification?

I invite you to explore one way to get alerts about the state of any appliance regardless of its _smartness_ based on signal from a binary sensor. In this example I'm attaching an SW-420 vibration sensor to the back of a washer and dryer to enable remote notification based on laundry room activity. 

While this guide centers around washer and dryer units, this concept may be extended to other appliances. For instance, you could use a thermocouple to send an alert when the oven is ready, etc.

To follow this guide you will need [Home Assistant](https://www.home-assistant.io/) running on the same network as the sensor unit.

> ## [Home Assistant](https://www.home-assistant.io/)
> Open source home automation that puts local control and privacy first.

There are other ways (IFTTT, Slack, Twitter) to get alerts from a sensor to your phone but these are a great topic for [another excellent guide](https://github.com/Shmoopty/rpi-appliance-monitor).

## Disclaimer
This guide assumes that the reader has intermediate knowledge and experience with electronics prototyping, soldering, scripting, debugging, and safe practices. 
As with any project there are many ways to acheive the desired result and this is just one of them. 
While I hope this is helpful, I offer no warranty and assume no liability for the result of following any or all of the instructions in this guide.

### [Step 1: Set up the ESP32 with ESPHome](https://github.com/rmooreID/Home-Assistant-Appliance-Monitor/blob/master/README.md#step-1-set-up-the-esp32-with-esphome-1)
Get started by installing the ESPHome Hass.io add-on. The [ESPHome Getting Started guide](https://esphome.io/guides/getting_started_hassio.html) assumes some knowledge of [Home Assistant which has it's own Getting Started guide](https://www.home-assistant.io/getting-started/).

### [Step 2: Set up the hardware](https://github.com/rmooreID/Home-Assistant-Appliance-Monitor#step-2-set-up-the-hardware-1)
Next, you'll want to get the hardware wired up. I decided to solder mine but you can use jumper wires if you prefer.

### [Step 3: Set up push notifications](https://github.com/rmooreID/Home-Assistant-Appliance-Monitor#step-3-set-up-push-notifications-1)
Finally, it's time to get push notifications.

## Here's what you'll need
- Home Assistant running on the local network
- Home Assistant companion app running on iOS
- [ESP32](https://learn.adafruit.com/adafruit-huzzah32-esp32-feather)
- SW-420 Normally Closed Vibration Sensor Module
- 22AWG-28AWG stranded wire (either pre-terminated [jumper wires](https://www.adafruit.com/?q=jumper%20wires) with BLS connectors or using a BLS connector kit and crimping tool)
- [half-size breadboard](https://www.adafruit.com/product/64)
- USB cable
### Optional
- Soldering Kit
- [Perfboard](https://learn.adafruit.com/collins-lab-breadboards-and-perfboards/learn-more)
- Dust-resistant enclosure for SW-420 (I used some polypropylene cases I had sitting around)


## Step 1: Set up the ESP32 with ESPHome
1. Connect ESP32 and SW-420 to a breadboard. The red LED next to the USB port on the [Huzzah32](https://learn.adafruit.com/adafruit-huzzah32-esp32-feather/pinouts) is directly connected to GPIO#13. I decided to plug in the data line there to see if things were registering on the ESP32. The LED on the ESP32 started blinking when I gave the board a gentle tap.
![ESPHome Dashboard](./assets/laundrybot-0.jpeg)
2. Install [Home Assistant](https://www.home-assistant.io/getting-started/)
3. Install [ESPHome](https://www.home-assistant.io/components/esphome/) and continue through the steps to set up your first node.
4. Compile and upload [LaundryBot.yaml](./LaundryBot.yaml) script to ESP32 using [ESPHome](https://esphome.io/guides/getting_started_hassio.html)
You may have noticed that I have this file set up for two different sensors connected to the same ESP32. If you only need one sensor you can delete the second one.
```YAML
LaundryBot

esphome:
 name: laundrybot
 platform: ESP32
 board: featheresp32

wifi:
 ssid: 'network-id'
 password: 'network-password'

api:
 password: 'api-password'

ota:
 password: 'ota-password'

binary_sensor:
 - platform: status
   name: "LaundryBot"
 - platform: gpio
   pin: GPIO13
   name: "washer"
   device_class: vibration
   filters:
   - delayed_on: 10ms
   - delayed_off: 5min
```
![ESPHome Dashboard](./assets/laundrybot-9.jpeg)
5. Once you've tested the hardware you may find that you need to fine tune the [Binary Sensor Filter](https://esphome.io/components/binary_sensor/index.html?highlight=binary%20filter#binary-sensor-filters) which helps debounce the input signal and potentially mitigate false positives. 
```YAML
   filters:
   - delayed_on: 10ms
   - delayed_off: 5min
```

## Step 2: Set up the hardware
1. I had originally planned on terminating the wires with BLS connectors and connecting the female BLS connector directly to the SW-420. I started to wonder if this type of connection in a vibration-intensive environment would introduce unnecessary risk. Ultimately I decided to solder the connections instead.
![ESPHome Dashboard](./assets/laundrybot-1.jpeg)
2. Optionally, cut appropriately sized rectangles of [perfboard](https://learn.adafruit.com/collins-lab-breadboards-and-perfboards/learn-more) and solder the sensor wires and the SW-420 to the perfboard.
![ESPHome Dashboard](./assets/laundrybot-2.jpeg)
3. Connect SW-420 sensors to ESP32.
![ESPHome Dashboard](./assets/laundrybot-4.jpeg)
4. Install sensor in the optional enclosure and attach them to the appliance you want to monitor.
![ESPHome Dashboard](./assets/laundrybot-5.jpeg)

## Step 3: Set up push notifications
1. Configure the sensor in the Integrations tab of Home Assistant.
![ESPHome Dashboard](./assets/laundrybot-17.jpeg)
2. Create a basic automation from the Automation menu within the Configuration tab.
![ESPHome Dashboard](./assets/laundrybot-15.jpeg)
3. Once the automation is created, edit the automation in the [automations.yaml](./automations.yaml) file, replacing _yourdevice_ with your iOS device name.
```YAML
- id: 'id-goes-here'
  alias: Washer is done!
  trigger:
  - entity_id: binary_sensor.washer
    from: 'on'
    platform: state
    to: 'off'
  condition: []
  action:
  - service: notify.ios_yourdevice
    data:
      message: Washer is done!
```
4. Try to trigger the sensor and adjust the sensitivity control on the SW-420 until the desired threshold is acheived.
![ESPHome Dashboard](./assets/laundrybot-12.jpeg)
5. :tada: If everything is working, you should be able to receive notifications in the Home Assistant iOS app.
![ESPHome Dashboard](./assets/laundrybot-20.jpeg)


> Copyright 2019 Ryan Moore

   Licensed under the Apache License, Version 2.0 (the "License");
   you may not use this file except in compliance with the License.
   You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

   Unless required by applicable law or agreed to in writing, software
   distributed under the License is distributed on an "AS IS" BASIS,
   WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
   See the License for the specific language governing permissions and
   limitations under the License.
