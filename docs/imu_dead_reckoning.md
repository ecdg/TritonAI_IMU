# IMU Dead Reckoning
1. [IMU Dead Reckoning from the Triton AI Racer platform](https://github.com/Triton-AI/Triton-AI-Racer-ROS2/blob/96c5d9303b7f1d88dabf7ae9ceb214741d41f20d/src/interface/donkeysim_tai_interface/donkeysim_tai_interface/donkeysim_client_node.py#L244)

IMU Dead Reckoning for pose estimation utilizes an object's previously known position and its estimated speeds over elapsed time. One way to do this is by integrating the acceleration data, taken from the IMU's accelerometer, over time to get velocity, then integrating the estimated velocity to get an estimated position. To find an object's orientation, we can use the angular rates taken from the IMU's gyroscope—integrating them using quaternions.

IMU Dead Reckoning is useful when GPS data is unavailable. However, please take into account the gradual accumulation of error, which we call "drift".
