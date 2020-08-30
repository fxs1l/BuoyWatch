# BuoyWatch
![buoywatch_logo.png](https://github.com/fxs1l/Buoywatch/blob/master/images/buoywatch_logo.png "BuoyWatch Logo")

The marine environment has served as a crucial part in the lives of many. It is the home of a large number of creatures which have contributed to the source of food and livelihood for numerous people. However, the marine life is currently facing problems with illegal fishing, where vessels use prohibited methods in fishing thus damages reefs and other vulnerable marine ecosystems. Therefore there is an urgent demand for a new approach in detecting, reporting and analyzing illegal fishing within our community.

BuoyWatch is a two-node device. The first node is a [detection device](https://github.com/fxs1l/buoywatch-detector) which will be deployed via a buoy. It will be responsible for detecting fishing vessels located within 15-km near the coastline. It has (1) a camera tasked with monitoring and detecting fishing vessels through object detection and (2) a light sensor tasked with detecting sudden changes in illuminance underwater at night. The second node is a central device which will receive the data transmitted by the detection system  and will be sending an SMS text message to the authorities through the Twilio Library and notification from the BuoyWatch mobile application regarding the detection, together with the coordinates of the fishing vessel. 

## Mobile Application
Along with this device is a [mobile application](https://github.com/RaphCondor/buoy-watch-app), linked through WiFi/Bluetooth connectivity, that will contain updates of the deployed node and the collected parameters. The software will be used as a database where current and previous records of illegal fishing vessels can be found.

<img src="https://github.com/fxs1l/Buoywatch/blob/master/images/app.png" width="400" > 
<img src="https://github.com/fxs1l/Buoywatch/blob/master/images/detected.png" width="400" >
<div style="text-align:center"><img src="https://github.com/fxs1l/Buoywatch/blob/master/images/location.png" width="400" ></div>

## Boat Detection Machine Learning model
The boat detection machine learning model can be accessed [here](https://github.com/fxs1l/buoywatch-detector). A total of 1000  varied images were found with the search parameters of “boats”, “bangka”, “yachts”, and “ships”. The selection of photos were without any biased or purposeful influence. 80\% of the images was selected for the training data and the remaining 20\% was for the validation. No image augmentations were made on the dataset kay we used a model nga pretrained on a larger dataset. Annotations were done on all images to specify the certain regions with seacraft appearance. This was done through labelImg which exports the annotations in YOLO format: <object class> <x> <y> <width> <height>. 
  
Using [YOLO: YOU ONLY LOOK ONCE (yolov5s.pt)](https://github.com/ultrayltics/yolov5) and taking advantage of its lightweight property which makes it convenient for the Raspberry pi environment, the seacraft detection model was trained for 200 epochs on the gathered dataset. The images chosen for validation were utilized in testing the model. For accuracy, an IOU (Intersection over Union)  threshold of 0.5 has been set. This evaluation metric is used for accuracy computation of a model for object detection on a particular dataset. For further analysis, the model was tested Precision and Recall. 


<img src="https://github.com/fxs1l/Buoywatch/blob/master/images/1.png" width="300" > 
<img src="https://github.com/fxs1l/Buoywatch/blob/master/images/2.png" width="300" >

Seen here are some examples of the seacraft detection of the machine learning model we have developed. The camera functionalities were accessible through the picamera python module. The two modes present are Video mode (5 seconds) and Burst shots. These are utilized as the two means of input for data. The boat detection python script begins by loading the boat detector model. The input data from the camera is then processed through the model. The model performs non-max suppressions on the predictions to ensure that each object is detected only once. The number of seacrafts calculated will be sent to the receiver if value is greater than 0. This process is encoded as a loop. Below is a graph of the parameters when training the model. It has a high precision, with an accuracy of 88.58%.

<div style="text-align:center"><img src="https://github.com/fxs1l/Buoywatch/blob/master/images/res.png" width="400" ></div>

## Design
For the first node of BuoyWatch, it will be deployed in a buoy. Proposed designs of the buoy were done in Fusion 360 to showcase the feasibility and structure of the marine buoy. It will house a 360 enclosure for the camera to rotate and detect seacrafts and boats within the area. It also has 4 x light dependent resistors to determine the presence of fishermen during the night, using light to attract fish to catch. BuoyWatch will be self sustainable, having solar panels, it will be able to charge itself under the sun.

<img src="https://github.com/fxs1l/Buoywatch/blob/master/images/buoywatch-detection-device.png" width="400" >


