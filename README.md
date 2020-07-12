# Home Automation Configuration
This repo contains the configuration and notes around my home automation setup for my Apartment using Home Assistant and a few other technologies.

The original configuration I had before I renovated my apartment can be found in the version 1 tag of this repo.

## Equipment

I currently have the following equipment configured.

| Component | Function |
|------------|----------|
| <ul><li>Intel NUC Mini PC</li></ul> | <ul><li>Model: BOXNUC7CJYSAL4 (4GB + 32GB)</li><li>Runs Ubuntu</li><li>Runs Home Assistant Core</li></ul> |
| <ul><li>USB Zigbee Stick</li></ul> | <ul><li>Provides Zigbee capability to Home Assistant</li></ul> |
| <ul><li>Phillips HUE Basestation</li></ul> | <ul><li>Hue functionalut</li><li>Phillips HUE Bulbs</li><li>Light Strips</li><li>Hue Switches</li><li>Movement sensors</li><li>IKEA Tradfi downlights</li></ul> |
| Xiaomi Aquara | <ul><li>Door and Window Sensors</li><li>Temp and Humidity Sensors</li><li>Motion sensors</li><li>Smart Power Plugs</li></ul> |
| Xiaomi Rockrobo Vacuum Cleaner | <ul><li>Automated claning system</li></ul> |
| Xiaomi Flower sensors | <ul><li>Automated claning system</li></ul> |
| Xiaomi Cameras | <ul><li>Automated claning system</li></ul> |
| Daikin Air Conditioner | <ul><li>Automated claning system</li></ul> |
| Broadcom IR Blaster | <ul><li>Automated claning system</li></ul> |
| Google Home and Home Assistant Cloud | <ul><li>Automated claning system</li></ul> |
| Unifi Network equipment | <ul><li>Automated claning system</li></ul> |



Changes
* Moved off the Pi3
* Moved off the Xiaomi hub to native support in Zigbee

Integration image


### Xiaomi Rockrobo Vacuum Cleaner
I invested in this for the novelty factor, however given my apartment is a single level, it's actually amazing at keeping my place tidy, much more than I expected.

There are a few ways to get it integrated with Home Assistant, however the most robust way that I have found is to root it and extract the API key.

To root it, check out the presentation these guys did at CCC -  [34C3 - Unleash your smart-home devices: Vacuum Cleaning Robot Hacking](https://www.youtube.com/watch?v=uhyM-bhzFsI),and then I used their docs to root my vaccuum, and run the following commands to extract the API token to integrate it into Home Assistant.

```
root@rockrobo:~# printf $(cat /mnt/data/miio/device.token) | xxd -p
```

You can find more about this command on this github issue: https://github.com/dgiese/dustcloud/issues/12 and there is a good review of this model on youtube here: [Xiaomi Mi Robot Vacuum Review - Raising The Bar at an Affordable Price
](https://www.youtube.com/watch?v=eWLWO5AxAHo)

Once this is configured, you can also voice control it using the google assistant / cloud integration as it now exposes the vacuum domain. 

### Xiaomi Flower sensors
As I mentioned above, I use the Xiaomi sensors to get some light , tempreature and other metrics for around the chilli plants that I grow both indoors and out, however one thing I found was that using these sensors really wasn't suitable for my gardening requirements, and so I picked up a few of the "Mi Flora" plant sensors.

These are now owned by Xiaomi, and I don't think they are fully integrated with the Mi Home application as yet (and may never be) , and as such they use their own application to configure and use them. To say the Mi Flora application is garbage is an understatement, and gave up trying to reliably use it, opting to integrate it directly with Home Assistant on the Pi3 using the below documentation. This has been extremely stable and providing much more metrics about the health of the plants including moisture, light and fertility.

* https://www.home-assistant.io/components/sensor.miflora/

It is worth noting that these devices communicate using Bluetooh Low Energy rather than Zigbee, so your Home Assistant platform must have a Bluetooth interface, for those running HASS on the Pi3 this does just work.

### Temperature and Humidity sensors
I have a number of the temprature and humidity sensors throughout my apartment which enables me to understand more about the temprature of the rooms.

Given the automation capabilities of Home Assistant, its quite simple to have automations using these sensors in which notifications or climate control actions trigger based on certain temprature events.

### Xiaomi Cameras
I have a set of 2 Xiaomi Dafang	cameras, that i have running custom firmware.

I haven't used these cameras without the custom firmware as they tie in to the Xiaomi cloud / Mi Home ecosystem and i'm not overly thrilled with the security and privacy implications. It's pretty easy to flash the custom firmware yourself if you are relatively familiar with Linux.

I got in early on these cameras when the Firmware was quite experimental, and it's quite impressive to see how much the custom firmware has evolved, it's support for movement, SSL and MQTT is really quite impressive. I currently use them with the ffmpeg camera support, and will likely move them over to MQTT later on once I have my head around it.

More information about these cameras and the custom firmware can be found at the links below;

* [Hacking a $25 IOT Camera to do more than it's worth](https://hackernoon.com/hacking-a-25-iot-camera-to-do-more-than-its-worth-41a8d4dc805c)
* [Xiaomi Dafang Hacks - Github.com](https://github.com/EliasKotlyar/Xiaomi-Dafang-Hacks)

### Daikin Air Conditioner
Unbeknownst to me, this was actually my very first IOT purchase. When adding this Daikin AC unit to my home to help me survive the brutal Sydney summer heat, I opted to get the wifi controller to be able to control it from an application both locally and remotely. 

I expected this to be pretty locked down, and unlikely to be able to be integrated into a wider automation platform and did some poking around online and found that the local wifi communication between android and iphone applications is unauthenitcated and in plaintext which in the context of IOT is generally bad, however the benefit of this is that the API has already been extensively reverse engineered and a Python interface for it built which has been dropped into Home Assistant, making it a first class citizen that "just works" via auto discovery.

To make things even better, recent changes and enhancements with this component have made it work extremely well with the Google Home climate control capability in the home application as well as voice control (I'd suggest signing up for the home assistant cloud service to make things a little easier to get working).

There is a number of related projects that I used as references when trying to understand how all this went together, and you can check them out below if you have this equipment.

* https://github.com/ael-code/daikin-control
* https://www.home-assistant.io/components/daikin/
* https://pypi.org/project/pydaikin/
* https://github.com/Apollon77/daikin-controller


### Broadcom IR Blaster
I have a Broadcom Mini 3 IR blaster that I use to turn on the Kambrook Tower Fan I have in my bedroom (KFA837 Arctic LED Display Tower Fan just incase you have this model and want to use my IR codes)

This is possible because I've loaded the IR remote codes for the fan into home assistant, then trigger them as a switch to turn it on.

What is great about this is that the switch domain is exposed to the Home Assistant cloud integration for google home, so I can now control it using google voice actions such as "OK Google - Turn on bedroom fan" and it works great.

* https://www.home-assistant.io/components/switch.broadlink/



### Google Home and Home Assistant Cloud

* 2 x Google Home Mini's
* 1 x Google Home
* 1 x Google Chromecast

Voice control via Home Assistant Cloud works a treat with vacuum, switches, light domains

Mainly use this as group speakers for podcastss and music









