This document introduces the development, coding tools, coding libraries for Intel OpenVINO EdgeAI.
---
# Coding platform
> **Local coding**
>> Hardware: https://software.intel.com/en-us/openvino-toolkit/hardware  
>> Software OpenVINO: https://software.intel.com/en-us/openvino-toolkit
>>> Document: https://docs.openvinotoolkit.org/latest/index.html  
>>> Toolkit: https://github.com/opencv/dldt Designed to accelerate the development of applications and solutions that emulate human vision. Based on (CNN), the toolkit extends workloads across Intel hardware (including accelerators) and maximizes performance.

> **Cloud coding** No hardware setup required on your end. The DevCloud utilizes Jupyter* Notebooks to execute code directly within the Web browser. https://devcloud.intel.com/edge/home  

# Deep learning networks
*	SSD
*	YOLO
*	Faster RCNN
*	MobileNet
*	ResNet
*	Inception
  
![OpenVINO Workflow](openvino-workflow.gif)  
Modles->Optimizer->IR->Inference Engine-> Extracting Results ->Edge deployment  

# Deep learning models
1. We need DL models to fetch into OpenVINO inference engine. Using a tool, such as Caffe, to create and train a CNN inference model or directly use the pretrained models from below link.  
https://software.intel.com/en-us/openvino-toolkit/documentation/pretrained-models  
https://github.com/opencv/open_model_zoo  
2. In oder to fetch the model into IE, we need adjust or resize the input data per model’s requirement and what library you use to load an image or frame, the requirement can be checked from model reference webpage or from OpenVINO Toolkit documentation.

