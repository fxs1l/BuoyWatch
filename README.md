# BuoyWatch
![buoywatch_logo.png](https://github.com/fxs1l/Buoywatch/blob/master/images/buoywatch_logo.png "BuoyWatch Logo")

## Introduction
The marine environment has served as a crucial part in the lives of many. It is the home of a large number of creatures which have contributed to the source of food and livelihood for numerous people. However, the marine life is currently facing problems with illegal fishing, where vessels use prohibited methods in fishing thus damages reefs and other vulnerable marine ecosystems. Therefore there is an urgent demand for a new approach in detecting, reporting and analyzing illegal fishing within our community.

BuoyWatch is a two-node device. The first node is a [detection device](https://github.com/fxs1l/buoywatch-detector) which will be deployed via a buoy. It will be responsible for detecting fishing vessels located within 15-km near the coastline. It has (1) a camera tasked with monitoring and detecting fishing vessels through object detection and (2) a light sensor tasked with detecting sudden changes in illuminance underwater at night. The second node is a central device which will receive the data transmitted by the detection system  and will be sending an SMS text message to the authorities through the Twilio Library and notification from the BuoyWatch mobile application regarding the detection, together with the coordinates of the fishing vessel. 

## Mobile Application
Along with this device is a [mobile application](https://github.com/RaphCondor/buoy-watch-app), linked through WiFi/Bluetooth connectivity, that will contain updates of the deployed node and the collected parameters. The software will be used as a database where current and previous records of illegal fishing vessels can be found.

<p float="left">
  <img src="https://github.com/fxs1l/Buoywatch/blob/master/images/app.png" width="200" />
  <img src="https://github.com/fxs1l/Buoywatch/blob/master/images/detected.png" width="200" /> 
  <img src="https://github.com/fxs1l/Buoywatch/blob/master/images/location.png" width="200" />
</p>

The mobile application was developed through Flutter, the Mobile App SDK of Google. It uses dart and is cross-platform, meaning it can run on both iOS and Android. GoogleMaps Library has been imported to display the Maps where the boats can be seen. The Boat detection buoy will send its coordinates in order for the authority to quickly locate where the illegal fishing has occured. The mobile application will gather the data from the central device through Bluetooth/WiFi within the Local Government Unit or where the authorities are. Future recommendations would be that the data be logged and stored in a web server so that data can be accessed anywhere as long as the app is connected to the internet.

## Illegal Fishing Boat Detection
### Boat Detection Machine Learning Model
The boat detection machine learning model can be accessed [here](https://github.com/fxs1l/buoywatch-detector). A total of 1000  varied images were found with the search parameters of “boats”, “bangka”, “yachts”, and “ships”. The selection of photos were without any biased or purposeful influence. 80\% of the images was selected for the training data and the remaining 20\% was for the validation. No image augmentations were made on the dataset kay we used a model nga pretrained on a larger dataset. Annotations were done on all images to specify the certain regions with seacraft appearance. This was done through labelImg which exports the annotations in YOLO format: <object class> <x> <y> <width> <height>. 
  
Using [YOLO: YOU ONLY LOOK ONCE (yolov5s.pt)](https://github.com/ultrayltics/yolov5) and taking advantage of its lightweight property which makes it convenient for the Raspberry pi environment, the seacraft detection model was trained for 200 epochs on the gathered dataset. The images chosen for validation were utilized in testing the model. For accuracy, an IOU (Intersection over Union)  threshold of 0.5 has been set. This evaluation metric is used for accuracy computation of a model for object detection on a particular dataset. For further analysis, the model was tested Precision and Recall. 

<p align="center">
  <img width="460" height="300" src="https://github.com/fxs1l/Buoywatch/blob/master/images/1.jpg">
  <img width="460" height="300" src="https://github.com/fxs1l/Buoywatch/blob/master/images/2.jpg">
</p>

Seen here are some examples of the seacraft detection of the machine learning model we have developed. The camera functionalities were accessible through the picamera python module. The two modes present are Video mode (5 seconds) and Burst shots. These are utilized as the two means of input for data. The boat detection python script begins by loading the boat detector model. The input data from the camera is then processed through the model. The model performs non-max suppressions on the predictions to ensure that each object is detected only once. The number of seacrafts calculated will be sent to the receiver if value is greater than 0. This process is encoded as a loop. Below is a graph of the parameters when training the model. It has a high precision, with an accuracy of 88.58%.

<img align="center" src="https://github.com/fxs1l/Buoywatch/blob/master/images/res.png" alt="Results">

### Light Sensor Detection
During at night, since the RPI Camera for Boat detection will have a hard time detecting boats, the Light Dependent Resistors (LDR) will take its place. Since fishermen use light to attract fish at night, the Light Sensors have been calibrated that if a significant amount of illuminance/brightness are detected near the deployed buoy, illegal fishing may be present within the area. This will be sent to the Local Government Unit or to the authorities for reporting.

### Scheduling Feature
Nighttimes  are  noted  based  on  timezones  via  Astral  Python  Module.    Theseacraft  detector  Pi  System  is  allowed  the  capability  of  distinguishing  nightfrom day through these verifications.  Through this,  successful automation oflight detector operation for nighttime and boat detector operation for daytimeis done.

## Design
For the first node of BuoyWatch, it will be deployed in a buoy. Proposed designs of the buoy were done in Fusion 360 to showcase the feasibility and structure of the marine buoy. It will house a 360 enclosure for the camera to rotate and detect seacrafts and boats within the area. It also has 4 x light dependent resistors to determine the presence of fishermen during the night, using light to attract fish to catch. BuoyWatch will be self sustainable, having solar panels, it will be able to recharge itself under the sun. The condition of the deployed buoy can also be seen on the mobile application whether or not the sensors need maintenance.

<p float="left">
  <img src="https://github.com/fxs1l/Buoywatch/blob/master/images/buoywatch-detection-device.png" width="400" />
  <img src="https://github.com/fxs1l/Buoywatch/blob/master/images/rpi.png" width="400" />
</p>

# How It Works
Buoy  Watch  is  a  monitoring  system  focused  on  marine  life  and  environment.It  is  made  up  of  three  parts:  (1)  a  Boat  Detector,  (2)  a  Data  Receiver,  and(3) a mobile application.  This project utilizes radio frequency transmitters forwireless transmissions of data from and towards each device.

1. Boat Detector
   - This device is located on-site.  It is composed of a Raspberry Pi cam-era and a light sensor.  Each component functions on different periodsof the day.  In daytime, the Raspberry Pi camera takes images andchecks for boats present in the area through a trained machine learn-ing  model.   Once  detection  is positive,  the  device  saves  the  image.In nighttime, the light sensor detects light used by fishermen.  Posi-tive detection is saved.  Data from both components are sent to (2)through radiofrequency modules.

2. Data Receiver
   - This  unit  is  a  Raspberry  Pi  (model)  located  on  a  Local  Govern-ment Unit site.  It receives data from the Boat Detector.  Converting received computations for more suitable arrangements, the receiversends data to the mobile application via Bluetooth.

3. Mobile Application
   - This application is designed to immediately signal the authorities asdata are received through messages.  As the bluetooth setting of amobile phone activates, the app instantly carries over the data.  Thisallows a swift and convenient delivery of such findings.

## Conclusion
With  an  accuracy  level  of  88.58%,  the  device  proves  exceptionally  fair  per-formance  on  sea  environment  monitoring  through  boat  detecting  operations.Findings on the analysis of its precision and recall bring additional support tothe mentioned assertion with both values greater than 85%.  In addition,  thehandiness of an easy-to-use modelled application for mobile use boosts the con-venience level of the project with findings made accessible immediately.  Lastly,as it flashes a unique and practical design, this project features the embodimentof a complete and functional solution to depleting marine life and environment.

