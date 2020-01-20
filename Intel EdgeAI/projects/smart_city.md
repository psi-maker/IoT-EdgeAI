Conclude the structure, hardware, software, theory and optimization for https://github.com/incluit/OpenVino-For-SmartCity  
Intend to:  
* Try with different models  
*	Detect vehicles and pedestrians  
*	Draw Areas of Interest  
* Object Tracking  
*	Object Trajectories  
*	Fix labels for the other models  
*	Calculate objects velocities  
*	Calculate objects accelerations  
*	Detect collisions  
*	Elaborate on dangerous situations to be detected (near misses)  
*	Detect these situations  

# PLATFORM
	C++
	Raspberry Pi camera, Intel Core CPU with integrated graphics(I have not this equipment, I will use NVIDIA Jetson Nano integrated with GPU, ubuntu18.04 64-bit) 
	OpenVINO toolkit and its dependencies, OpenCV https://docs.openvinotoolkit.org/latest/index.html
	Install BOOST library
apt-get install libboost-dev
apt-get install libboost-log-dev
	Openvino inferrence pre-loaded examples: https://github.com/intel-iot-devkit/inference-tutorials-generic/tree/openvino_toolkit_r4_0
	Inference engine samples - (Yolo v3 object detection demo): https://software.intel.com/en-us/articles/OpenVINO-InferEngine
	Model Optimizer to convert Tensorflow models (Yolo v3): https://software.intel.com/en-us/articles/OpenVINO-Using-TensorFlow
	Tensorflow Yolo v3 repo: https://github.com/mystic123/tensorflow-yolo-v3
	Yolo v3 docs: https://pjreddie.com/darknet/yolo/
INSTALLATION CHECK
	OpenVINO™ toolkit can run the supplied demo samples
	to use a GPU: You have installed and tested the GPU drivers
	camera tested
	to use a Myriad: You have connected and tested the USB Intel Movidius Neural Compute Stick
	connected to network and has Internet access
	download all the files from GitHub
BUILD
https://github.com/incluit/OpenVino-For-SmartCity#build
ASYNCHRONOUS API 
To use of the Inference Engine asynchronous API to make multiple inference requests without waiting for results, run and stack different models synchronously or asynchronously https://github.com/intel-iot-devkit/inference-tutorials-generic/tree/openvino_toolkit_r4_0/car_detection_tutorial/step_4
 


THEORY
1.	Region of interest area
https://github.com/incluit/OpenVino-For-SmartCity/wiki/0---Areas-of-Interest
ROI can be categories as sidewalks, crosswalks, and street; Sometimes for special street, we will be able to differentiate its orientation (North, East, South, West).  One of the benefits of defining these areas of interest is that it will help us to focus and optimize the precision of the detection model into a specific space, reducing the margin of error (false positives, misdetection, etc). With these regions defined we can also trim or mask the original frames in order to reduce the computing times of the inference and future image processing.

In order to handle near misses scenarios, we need to draw an interest area and a set of rules is going to be defined by using these areas. For example: A Car MUST NOT be on a sidewalk; No car and pedestrians simultaneously on a crosswalk; A car MUST NOT have 0 velocity on a crosswalk.
Due to some limitations regarding the object detection modules, most of them related to external factors such as camera position, camera angle, video’s horizon line, etc, users should give the option of manually drawing the desired areas of interest, to customize the area of interesting according to camera conditions, for each particular case, improving the object detection model functionality.

On the next stage (detection and/or tracking), the rectangle where this process is being done will be slightly highlighted and no detection will be done outside of it.

2.	Object detection
https://github.com/incluit/OpenVino-For-SmartCity/wiki/1-Object-Detection
The inputs of this stage are the frames of the input feed. The outputs of this stage are the ROI over the desired objects, detected by deep learning recognition models. These models are optimized by Intel’s OpenVINO Toolkit.
We can use general detection model, such as Yolo(run the model optimizer of the Yolo model), or use a dedicated model for each set of objects (Vehicles, Pedestrians, ..) in parallel. The project supports:
	Single model - multiple object detection (person-vehicle-bike-detection-crossroad-0078)
	Dual model - dual object detection (vehicle-detection-adas-0002 and person-detection-retail-0013) this could still be improved to use more models.
	Yolo v3 detection model.
3.	Object Tracking
https://github.com/incluit/OpenVino-For-SmartCity/wiki/2-Object-Tracking
The inputs of this stage are the ROIs from the detection stage. The outputs of this stage are a new ROI over the tracked objects along with an ID that identifies it.
The basic idea is to use the detection models in order to identify objects on the input feed. run the inference process on each frame until have a detection. then add that ROI to the tracker system and start tracking it over the following frames. keep running the inference process every once in a while (a variable X amount of frame) to feedback the tracker with the new information:
1.	If there is a new object, add it to the tracking system.
2.	If the tracker lost the object, add it again.
3.	If tracking the new ROI has been moved away enough from the original object, reset the tracker for that object.
4.	If a detected object is about to exit the scene, remove it.
5.	If there is a lost object being tracked, remove it.
The Tracker.hpp and Tracker.cpp files implement the tracking system
4.	Collision Detection
https://github.com/incluit/OpenVino-For-SmartCity/wiki/3---Collision-Detection
Once the tracking working, we can retrieve for every object its actual and past positions, average the last 5 frames to filter the noise of the detection models. Once we have the position, we can get the velocity as the difference of two consecutive positions. Again, we calculate the average of the last 5 velocities to avoid abrupt changes that do not match what actually happens in the scene. 

This can be improved by shape matrix calculation: We also normalize this speed with a factor of 1/y (being y the vertical position of the object in the image). This has to be done because objects moving closer to the camera appear to be moving faster (in terms of pixels/frame) but that it is not true in reality. Once we get the velocities of each tracked object, we can calculate the acceleration analogously. 

After tracking system can identify an object over different frames, we can add properties to the detected objects. In order to detect near misses on a street crossing, it should be able to detect collisions.
Detection of overlapping ROIs (region of interest): Detection of overlapping ROIs is a nice feature to have in order to filter out collisions of objects that are not on the same plane. For the single camera, in order to prevent a false collision detection, and use the area of the ROI. If the ROIs areas are too different in size, there is no collision between the 2 objects. In order to calculate the object trajectory, it assume the object position as the center of its bounding box. Accumulating positions of previous frames and then find the average in order to filter small detection movements.
Velocity vector: To add a second verification for ROI, the velocity gradient (acceleration). A collision between 2 objects typically implies a major variation on the trajectory and the velocity of at least one of the objects. average position = instant speed. Taking into account time unit is a frame, calculate the speed as the difference of position of the object measured in two consecutive frames. average that speed in order to get a smooth representation. The main goal is to measure acceleration. The speed vector measured with an error if an object going a constant speed directly facing move near to the camera, due to the 2 dimensional perception of the camera. The speed should not constantly measured in pixel/frames, should not be 0. To correct this we are normalizing the position_y value with the 1/y factor.
Identify different cases of collisions: a vehicle is in a dangerous situation when there is a jump in acceleration. This jump as the difference between the current frame acceleration and the average of the previous three. We then determine if this difference is bigger than an arbitrary and empirical defined value and then consider it for a possible collision. 
Finally, if the ROI of an object marked in a dangerous situation overlaps with any other ROI, we consider it a near-miss. If the other object is also marked, we consider it a collision.
As an example of our analysis we can see the van’s speed and acceleration in the following image and identify the peaks in acceleration. The evolution of the speed matches the behavior of the van. It appears in the scene going downwards at a constant speed before crashing with the other car, then it brakes, slows down, and finally a sudden increase in speed occurs when the crash happens.
 
In the image above, we can assume that a white van goes at constant speed before getting to the intersection, we can see this plotted as a red line in the chart. Then, it tries to break (we can see a small down peak) and then the black car crashes into it, increasing its speed. The yellow line represents the acceleration of the van, we can clearly see that we could define a threshold on the acceleration to define a hard crash like this one.
5.	Near misses
Define near misses as an "extension" of collisions, in the way that we can identify thresholds in speed and acceleration and search for other vehicles in the neighborhood of the offender. We could then detect near misses. These situations and other dangerous ones are further explored in https://github.com/incluit/OpenVino-For-SmartCity/wiki/4-Near-Misses
Approach for detecting near misses consider the following scenarios:
	Cars that are going through an intersection (in perpendicular directions) and suddenly decrease their velocity and change their direction to avoid a collision.
	Cars approaching the crosswalk while pedestrians are crossing the street.
following scenarios will improve optimizing the Near Misses detection,:
	Extend the Areas of Interest interaction along with OpenVINO’s detection model, (i.e) by including "No Street Area" (for train rails) as AoI, and being able to detect when a vehicle has suddenly changed street lanes to the opposite direction.
	Detect when pedestrians are crossing the street without using the crosswalk.
6.	implemented Boost’s log module to register events on a unique file
7.	Dashboard
https://github.com/incluit/OpenVino-For-SmartCity/wiki/7---Dashboard
a Python library called dash(plotly) to develop the dashboard, which enables a side-by-side view of a collision/near-miss event and the representation of the internal variables of the tracked objects that played a part on those events. Database is MongoDB, which is a noSQL database document oriented.
8.	Dangerousness Metric
https://github.com/incluit/OpenVino-For-SmartCity/wiki/8-Dangerousness-Metric
a metric is used for PoC dangerousness after identifying those situations (collisions and near misses). Each event is registered and scored, has a different severity. Such as: 
a pedestrian not crossing on a crosswalk is not the same as a car crash. 
The number of detections (cars/pedestrians) should be reflected in the final value. 
A street where only 2 cars where detected and crashed should be more dangerous than one where 100 cars were detected and only 1 crash happened.
AWS CLOUD DEPLOY
AWS IoT framework can speed up development:
AWS IoT Core:
	AWS IoT certificates(encripted connection from thing to mqtt broker);
	Things management: groups and policies.
	MQTT broker
	AWS IoT actions and rules: redirect information to AWS elasticsearch service
AWS elasticsearch service: 
	Data stored and indexed throught elasticsearch
	Analytics with Kibana, and Cloud dashboarding was delivered using kibana near-realtime dashboards.
IMPROVEMENT BY DAVID
	At first set reasonable physical facility to slowdown speed on crossroad
	Put cameras for each road on the crossroad, directly face the road. Use the same edge devices to finish basic information calculation. (Conditions, object speed, object category, dangerous metrics, etc.)
	Calculated information can send to a hub device nearby, and do integration process. (Simulate the future and find possible dangerous)
	Object can receive the blinded area conditions or alert for simulated possible dangerous by pda or navigator
	Sen the picture immediately to police and hospital if accident is found 
