This document intend to analysis the structured design for AI-traffic, education, cooking, ninght vision. Focusing on their generic technology, I/O, hardware, software, network, etc.
---
# Smart Traffic
## Sensor fusion option
The best way for sensors installation is moveable.
* infrared thermal imaging sensors for pedestrain, animal, or other thermal objects: used for severe weather, less light  
* Dual vision camera: used for point cloud, three dimension process  
* Audio system: voice I/O  
* LiDAR: long distance
## Structure and Application
To avoid communication jam between equipments and useless data, sensors should be integrated with edge devices. Edge devices do basic AI analysis in high speed, send the result or possible alert information to central app/database for storage or further identification.  
  
Traffic is too different to use only DL, DL is the basic AI analysis such as: object category, road scenario etc. RL with evolution mechanism need be used for complex situation such as abnormal situation,possible accident prerequisite,the main purpose should be forcast near future and alert rather than only find accident.  
  
I think the important point for smart traffic system is the simulation backbone system, which is used for AR/VR/node.js future simulation per matrix world calculation, real world physical law, and weather condition, as well as integrate the edge devices' result, then feedback to edge devices and alert possible accident objects.  
  
# GARBAGE SORTING AND SEPARATION
Garbage sorting and separation system is simple to compare with other system, but the facility is a big problem. Following options are possible implementation for public facilities:  
1. pre-construction of garbage system underground when building a house  
2. A facility with vertical space rather than landscape orientation outdoor  
3. A manipulator on the facility can easily seperate different objects  
4. Maybe simple process facility can be integrated with the faciliy such as paper smash, liquid drying, or food smash, etc. This will simplify following transportation and process system cost.  

# AI Education
People have strong memories with picture and shape rather than sentence and words per the biological theory.  
One edge AI equipment with screen or AR/VR can recognize people talk with NLP, gathers related picture or cubic information from central servers, and make it visible will reinforce the learning curve or human communication.  
  
The most important part for AI education is the stereoscopic technology, NLP, vision or others consist with human brain running way.  

# Smart Cooking
# Sensors
* TellSpec equipment is a good reference for food ingredient detection, which use spectrometer scan to identify anaphylactogen,chemical substances, nutrient content and quantity of heat  https://tellspec.com/tw/  
* NLP is neccessary for communication with human

# Structure and Application
* Include the sensor to examine ingredients, the system also suggest the options of recipes on food materials by screen.   
* The best way for this system is to integrate with a health system, by that the system can suggest optimized food, recipes against everyone's physical body condition.  
* Even more, the system can control other kitchen equipment, such as the microwave timing, cooking timing etc.  

# AI Healthcare
I conclude some basic method for healthcare in this document. Healthcare and medical treatment are precision area that need zero error AI technology. Currently we need the development direction for AI healthcare should be man-machine integration rather than only dependent on robot.  

## Vision 
* fitness professionals, yoga, human body interface color, gongfu, dance with live feedbacks     
* Acute Myeloid & Lymphoblastic Leukemia Detection System. refer to https://github.com/AMLResearchProject/AML-ALL-Detection-System  
* Identifying Viral and Bacterial Pneumonia from Chest X-Ray Images    
* dynamically conceive the motion of a user's hand and ascertain the difference between intentional movement and unintentional movement  
* Invasive Ductal Carcinoma (IDC) Classification. refer to https://github.com/iotJumpway/Intel-Examples/tree/master/Intel-Movidius/IDC-Classification  
* Detecting caries(cavities) from an X-Ray image of a patient  
* iRiS is a preventive solution consisted of a mobile application connected to an independent, small and easy to use module equipped with an infrared thermal camera. iRiS will allow women to scan their upper body area and detect breast cancer (if present) at its early stages  
* detect and prevent oral cancer  
* Eye movement when reading could be an early indicator of Alzheimer's. So tracking the eye movements can help in predicting  Alzheimer's. refer to https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5332166/     
* Transradial myoelectric prosthesis that send use data gathered by Myo armband (Thalmic Labs). refer to https://www.instructables.com/id/ADAMS-Hand-a-Low-cost-Myoelectric-  

## Applications
* visually impaired in a world of complex ambient activities and interactions. It leverages the power of AI in Computer Vision to analyze real-time events and generate feedback in the form of audio to keep the user as aware as possible  
* Automated Doctor Machine. The machine has all the required input sensors inbuilt to check the condition of the patient and also provides a diagnosis report with medicines. And this machine can be installed easily anywhere and it can be easily accessed using patient's unique card. All the payment are done online after providing the medicines to the patients. After receiving the medicines, the patient's health report is updated to the health record in his cloud account. So when he meets the doctor in future, the doctor can easily get his up to date health record and the doctor can also use the webapp to update his health record  
* Detect the position of the patient and predict falls  

## Body Extension
* haptic response to the users' fingers with the help of electroactive polymers to help counteract the unwanted vibration  
* the interpreter which receives brain signals from neurons and the AI program interprets the signal and instruction sets in relation to these signals are sent to the second component which is the prosthetic limb or exoskeleton  
* Ultrasonic Sight is an electronic device for mobility, sociality and welfare of blind and visually impaired people  
* two small camera IR enabled be installed both side of the glasses, with the edge chip process vision image can give a new vision to blinded people，used for army night vision, or extend human vision capabilities

## Userful website for healthcare
'''
* www.auntminnie.com  
* Better eye health: www.peekvision.org  
* www.nhs.uk  
* digital health: www.dhealth.co.uk www.healthit.gov www.cms.gov connectopensource.atlassian.net apps.smarthealthit.org sharps-ds2.atlassian.net hl7.org/fhir   
* 3D printing tech. for healthcare: 3dheals.com enablingthefuture.org
* www.osirix-viewer.com  
* www.slicer.org open source software platform for medical image informatics, image processing, and three-dimensional visualization
'''

