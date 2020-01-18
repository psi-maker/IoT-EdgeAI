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
> Example of processing INPUT AND OUTPUT SHAPE  
For the model ‘human-pose-estimation-0001’ https://docs.openvinotoolkit.org/latest/_models_intel_human_pose_estimation_0001_description_human_pose_estimation_0001.html, we can get the output are two blobs with shapes: [1, 38, 32, 57] and [1, 19, 32, 57]. The first blob contains keypoint pairwise relations (part affinity fields), the second one contains keypoint heatmaps. Then what is the exactly mean for every dimension data on [1, 38, 32, 57] and and [1, 19, 32, 57]?    

> The model 'human-pose-estimation-0001’ is 
> * a multi-person 2D pose estimation network (based on the OpenPose approach) https://arxiv.org/pdf/1611.08050.pdf 
> * with tuned MobileNet v1 as a feature extractor https://arxiv.org/pdf/1704.04861.pdf
> * The pose may contain up to 18 keypoints: ears, eyes, nose, neck, shoulders, elbows, wrists, hips, knees and ankles
> * Source framework is caffe http://caffe.berkeleyvision.org/tutorial/net_layer_blob.html

> The conventional blob dimensions for batches of image data are number N x channel K x height H x width W. Blob memory is row-major in layout, so the last / rightmost dimension changes fastest. For example, in a 4D blob, the value at index (n, k, h, w) is physically located at index ((n * K + k) * H + h) * W + w.
> * Number / N is the batch size of the data. Batch processing achieves better throughput for communication and device processing. For an ImageNet training batch of 256 images N = 256.
> * Channel / K is the feature dimension e.g. for RGB images K = 3.  

> ‘human-pose-estimation-0001.xml’ showed the input dimensions on top of its reference webpage, and the ‘Mconv7_stage2_L2’ output dimensions at bottom of that page. Both have the same format [BxCxHxW]  

It shows as below picture.


