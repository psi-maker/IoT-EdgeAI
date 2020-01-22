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
  


