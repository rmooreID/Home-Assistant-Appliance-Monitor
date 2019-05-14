# LaundryBot
Get alarts about the state of a "dumb" appliance based on signal from a sensor. In this example I'm attaching an SW-420 vibration sensor to the back of a washer and dryer in a shared house to allow remote notification based on laundry room activity. 

While I'm using this for washer and dryer units, this concept may be extended to other "dumb" appliances. For instance, you could use a thermocouple to send an alert when the oven is ready, etc.

To follow this guide you will need [Home Assistant](https://www.home-assistant.io/) running on the same network as the sensor unit.
Home Assistant
> Open source home automation that puts local control and privacy first.

There are other ways (IFTTT, Slack, Twitter) to get alerts from a sensor to your phone but that's a topic for another guide.

## Here's what you'll need
- Home Assistant running on the local network
- ESP32
- SW-420 Normally Closed Vibration Sensor Module
- 22AWG stranded wire
- soldering kit
- mini breadboard
- plastic enclosure
- USB cable

## Step 1: Set up the software
1. Install Home Assistant https://www.home-assistant.io/getting-started/
2. Install ESPHome https://www.home-assistant.io/components/esphome/
3. Compile and upload .YAML script to ESP32 https://esphome.io/guides/getting_started_hassio.html

## Step 2: Set up the hardware
1. Solder SW-420 to breadboard
2. Terminate sensor wires
3. Connect ESP32 to SW-420 sensor
4. Install sensor enclosures and plug in ESP32

## Copyright 2019 Ryan Moore

   Licensed under the Apache License, Version 2.0 (the "License");
   you may not use this file except in compliance with the License.
   You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

   Unless required by applicable law or agreed to in writing, software
   distributed under the License is distributed on an "AS IS" BASIS,
   WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
   See the License for the specific language governing permissions and
   limitations under the License.
