# Folder Intel EdgeAI
## D1: Reviewd the Part2-Lesson 1, and make the detailed plan for 30 days intensive training content as below.
(1) Day1 – Day5: Review all content of this course, focus on industry scenario; OpenVINO structure, modules & workflow; course coding review; especially the ‘MQTT’
(2) Day6 – Day 10: Deploy one openVINO project 
(3) Day11 – Day 20: Conclude the structure from Intel IoT, AWS IoT and Building IQ webpages; conclude IoT architect from announced HR information database
(4) Day 21 – Day 30: Documentation from 1-4, especially understand the AI building and AI city

## D2: Part2-L2, L3 review & conclusion
- ‘Healthcare’has a large volume market in China for remote health examination
-  Within 2-3 years, China property management companies will popularize AI building and ‘Digital Security and Surveillance’
- ML has some consistency with ASK-SMELL-HEAR-DIAGNOSE of‘traditional Chinese medicine’and SHAPE-ACTION of‘kung fu’

## D3: Part2-L4 inference, L5 deploy review & conclusion
Intel OpenVINO workflow:
- Modle->Optimizer->IR->Inference Engine->Handle Result to get attribute values->edge deployment
- - Model Optimizer made some improvements to size and complexity of the models to improve memory and computation times
-- using IECore and IENetwork To load an IR into the Inference Engine
- - Inference Engine provides hardware-based optimizations to get even further improvements from a model. With synchronous(wait sequentially inference response) and asynchronous(Multi thread/process for failure, multi-frames, other tasks etc. ) function for vision inferencing.
-- edge devices are CPUs, including integrated graphics processors, GPUs, FPGAs, and VPUs
https://devmesh.intel.com/ can be used as the edgeAI project reference.
Tools to deploy Intel EdgeAI:
- OpenCV is an open-source library for various image processing and computer vision techniques that runs on a highly optimized C++ back-end; We use OpenCV for picture or video stream in,
- MQTT is a lightweight pub/sub architecture communication for resource constrined devices, communication etc. ROS also has this pub/sub service.
- we can use  FFmpeg library to transform video to server if necessary
- Node.js can be used for user front interface to show MQTT or FFmpeg data
Items for user requirement:
- balance accuracy, power, speed, size, cost
- Model fuse technology
## D4: 18, Jan. 2020 Coding library conclusion from couse; improve MQTT knowledge; 
- create a github repository for knowledge sharing 
-- DevOps.md introduces the development, coding tools, coding libraries for Intel OpenVINO EdgeAI
-- IoTcomm.md shows the information for IoT communication and protocol

## D5: 19, Jan. 2020 Find a projects to deploy from D6-D10
+ ‘Intel EdgeAI/models.md’: conclusion for Intel OpenVINO pretrained models, OpenVINO Open Model Zoo repository, and Intel IoT project reference webpage.
+ Decided to deploy the AI city project https://github.com/incluit/OpenVino-For-SmartCity in my laptop.
This document is the conclusion for Intel OpenVINO pretrained models, OpenVINO Open Model Zoo repository, and Intel IoT project reference webpage.  

## D6 20, Jan. 2020 
+ Conclude the structure, hardware, software, theory and optimization /Intel EdgeAI/projects/smart_city.md

## D7 21, Jan. 2020 Basic mathematics for DL, and YOLO analysis 
+ Related mathematics theory for YOLO framework math.md
+ YOLOv1 to YOLOv3 structures and evolution YOLO.md

## D8：22, Jan. 2020 Structured design and possible solutions for AI-traffic, education, cooking, ninght vision for future products’ vision. brain_storm.md

