# BuoyWatch
![buoywatch_logo.png](https://github.com/fxs1l/Buoywatch/blob/master/images/buoywatch_logo.png "BuoyWatch Logo")

The marine environment has served as a crucial part in the lives of many. It is the home of a large number of creatures which have contributed to the source of food and livelihood for numerous people. However, the marine life is currently facing problems with illegal fishing, where vessels use prohibited methods in fishing thus damages reefs and other vulnerable marine ecosystems. Therefore there is an urgent demand for a new approach in detecting, reporting and analyzing illegal fishing within our community.

BuoyWatch is a two-node device. The first node is a [detection device](https://github.com/fxs1l/buoywatch-detector) which will be deployed via a buoy. It will be responsible for detecting fishing vessels located within 15-km near the coastline. It has (1) a camera tasked with monitoring and detecting fishing vessels through object detection and (2) a light sensor tasked with detecting sudden changes in illuminance underwater at night. The second node is a central device which will receive the data transmitted by the detection system.  and will be sending an SMS text message to the authorities through a GSM Module regarding the detection, together with the coordinates of the fishing vessel. 

## Mobile Application
Along with this device is a mobile application, linked through WiFi connectivity, that will contain updates of the deployed node and the collected parameters. The software will be used as a database where current and previous records of illegal fishing vessels can be found.

## Design
<img src="https://github.com/fxs1l/Buoywatch/blob/master/images/buoywatch-detection-device.png" width="100" height="100">


