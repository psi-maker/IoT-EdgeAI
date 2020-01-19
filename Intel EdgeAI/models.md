Optimized models can expedite development and improve image processing pipelines.  
* [OpenVINO toolkit pretrained models](https://software.intel.com/en-us/openvino-toolkit/documentation/pretrained-models)  
* [Detailed info. for OpenVINO Pretrained Models](https://docs.openvinotoolkit.org/latest/_docs_resources_introduction.html)  
* [OpenVINO Toolkit Open Model Zoo repository](https://github.com/opencv/open_model_zoo)  
* [Intel IoT developer's tools, templates, libraries, DL project reference](https://devmesh.intel.com/topics/31)  
![How to use models](use_models.GIF)   
# OpenVINO toolkit pretrained models
> **IMAGE CLASSIFICATION**: A form of inference in which an object in an image is determined to be of a particular class, such as a cat vs. a dog.  

> **OBJECT DETECTION**: A form of inference in which objects within an image are detected, and a bounding box is output based on where in the image the object was detected. Usually, this is combined with some form of classification to also output which class the detected object belongs to.  

> **SEMANTIC SEGMENTATION**: A form of inference in which objects within an image are detected and classified on a pixel-by-pixel basis, with all objects of a given class given the same label.  
>> Semantic segmentation includes two steps:  
>> 1)	Location: frame objects by bounding box. bounding box usually contains 4 integer,-the topleft(x,y) and the bottom right(x,y); or the topleft(x,y) and the width, height  
>> 2)	Classification: classify the object in bounding box. CNN model scan the picture by kernel to extract the image feature, pooling the feature matrix and output for next layer process kernel, by this loop, the last full convolution layer will convert the feature matrix to probability.  

> **INSTANCE SEGMENTATION**: Similar to semantic segmentation, this form of inference is done on a pixel-by-pixel basis, but different objects of the same class are separately identified.  

## 

