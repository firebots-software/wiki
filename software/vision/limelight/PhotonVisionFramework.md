# PhotonVision Framework

## PhotonVision Subsystem

To use the subsystem you have to use the getLatestResult() to gt the pipeline. To get data n the target, use .getBestTarget on the result/pipeline. There are lots of methods under the result, so please refer to the photonvision docs in order to see them all. We will be using getPitch for the area.


## Photon Info

 We created a class called PhotonInfo that is able to return all the important information such as target distance from camera (meters) and print all the information that the camera returns. There are some static fields such as target height, camera height,  and camera yaw. Please ensure that the target height is not equal to the camera height as the distance will always return 0.0 if this is true. Another thing to notice is that there if a function called calibrateCameraAngle, and this will create multiple experiemental camera angles, and return the coresponding distance to tag. Use the actual distance from tag to find the right angle. 


 ALL MEASUREMENTS HERE ARE DONE IN METERS, SO PLEASE USE METRIC UNITS. 

