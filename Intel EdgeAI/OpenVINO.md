Intel OpenVINO workflow:
* Modle->Optimizer->IR->Inference Engine->Handle Result to get attribute values->edge deployment  
   ** Model Optimizer made some improvements to size and complexity of the models to improve memory and computation times  
   ** using IECore and IENetwork To load an IR into the Inference Engine   
   ** Inference Engine provides hardware-based optimizations to get even further improvements from a model. With synchronous(wait sequentially inference response) and asynchronous(Multi thread/process for failure, multi-frames, other tasks etc. ) function for vision inferencing.   
   ** edge devices are CPUs, including integrated graphics processors, GPUs, FPGAs, and VPUs 
https://devmesh.intel.com/ can be used as the edgeAI project reference. 

Tools to deploy Intel EdgeAI: 
* OpenCV is an open-source library for various image processing and computer vision techniques that runs on a highly optimized C++ back-end; We use OpenCV for picture or video stream in. 
* MQTT is a lightweight pub/sub architecture communication for resource constrined devices, communication etc. ROS also has this pub/sub service. 
* we can use  FFmpeg library to transform video to server if necessary 
* Node.js can be used for user front interface to show MQTT or FFmpeg data 

Items for user requirement: 
* balance accuracy, power, speed, size, cost 
* Model fuse technology
