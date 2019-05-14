# LaundryBot
Get alarts about the state of a "dumb" appliance based on signal from a sensor. In this example I'm attaching an SW-420 vibration sensor to the back of a washer and dryer in a shared house to allow remote notification based on laundry room activity. 

While I'm using this for washer and dryer units, this concept may be extended to other "dumb" appliances. For instance, you could use a thermocouple to send an alert when the oven is ready, etc.

To follow this guide you will need Home Assistant (HA) running on the same network as the sensor unit. HA is open-source and free to use. There are other ways (IFTTT, Slack, Twitter) to get alerts from a sensor to your phone but that's a topic for another guide.

## Here's what you'll need
- Home Assistant running on the local network
- ESP32
- SW-420 Normally Closed Vibration Sensor Module
- 22AWG stranded wire
- soldering kit
- mini breadboard
- plastic enclosure
- USB cable

## Step 1: Set up HA
