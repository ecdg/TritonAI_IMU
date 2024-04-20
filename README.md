# Using an IMU
## Creating an IMU package (on DonkeyCar—has some overlap with the process of creating an IMU package on ROS2)
### A script handling the data from the IMU
The standard DonkeyCar installation includes support for the **MPU-6050** and **MPU-9250** IMU sensors. They have [this script](https://github.com/autorope/donkeycar/blob/main/donkeycar/parts/imu.py) for handling the data from these IMU sensors.

For the **SparkFun 9DoF Razor IMU**, we can use [this script](https://github.com/NikitaB04/razorIMU_9dof/blob/main/imu.py).

For the **BNO086 IMU of the OAK-D Pro camera**, we have this [modified script](https://github.com/rohanmeserve/DSC190_WI24_Team_1_donkeycar/blob/main/DonkeyGPS/imu_oakd.py) adding support for this specific IMU sensor. These documents [(1)](https://docs.luxonis.com/projects/api/en/latest/components/nodes/imu/#)[(2)](https://docs.luxonis.com/projects/api/en/latest/samples/IMU/imu_accelerometer_gyroscope/#imu-accelerometer-gyroscope) were used as a guide by the DSC folks when creating the modified script.

If you are using a different IMU sensor, then look into its support documents for handling its data and whether or not they provide a script that does this. If there is none, then you can use its documents as a reference as well as other IMU scripts as a guide when creating one.

### To run on DonkeyCar
First, import the IMU script, e.g. imu.py, onto the car folder. Next, modify the complete.py file (it should be in the car folder) so that it imports the IMU object from the script. Then, edit the manage.py file so it imports from the modified complete.py file, which includes an ``add_imu``.

For how it looks:
- The change in complete.py, where the IMU object is imported from (highlighted section)
  ![modified_complete](https://github.com/ecdg/TritonAI_IMU/blob/main/docs/modified_complete_file.png)

- Adding a new part to DonkeyCar in manage.py: import object from file, instantiate object, V.add(object, inputs, outputs)
  ![add_part](https://github.com/ecdg/TritonAI_IMU/blob/main/docs/add_part_manage_file.png)

- If you decide to create a **_separate_** complete.py file, then make sure to change the import path in manage.py (highlighted section)
  ![separate_complete](https://github.com/ecdg/TritonAI_IMU/blob/main/docs/if_separate_complete_file.png)


## Pose estimation using an IMU sensor
Here are implementations of pose estimation using an IMU done in the Triton AI lab:
### IMU and GPS Sensor Fusion
1. [IMU and GPS sensor fusion using an Extended Kalman filter (EKF)](https://github.com/Triton-AI/DSC190_SP23_Team_EKFIMU) by DSC 190 students from Spring 2023
2. [IMU and GPS sensor fusion without EKF, relying on accurate RTK GPS data](https://github.com/rohanmeserve/DSC190_WI24_Team_1_donkeycar/tree/main) by DSC 190 students from Winter-Spring 2024, see its accurate estimation performance on this [notebook](https://github.com/rohanmeserve/DSC190_WI24_Team_1_donkeycar/blob/main/DonkeyGPS/notebooks/estimator_performance.ipynb)

(To-do) For more information on these approaches, please refer to the following documents: [fusion using EKF](), [fusion relying on RTK GPS]()

### IMU Dead Reckoning
1. [IMU Dead Reckoning](https://github.com/Triton-AI/Triton-AI-Racer-ROS2/blob/96c5d9303b7f1d88dabf7ae9ceb214741d41f20d/src/interface/donkeysim_tai_interface/donkeysim_tai_interface/donkeysim_client_node.py#L244) from the Triton AI Racer platform

IMU Dead Reckoning for pose estimation utilizes an object's previously known position and its estimated speeds over elapsed time. One way to do this is by integrating the acceleration data, taken from the IMU's accelerometer, over time to get velocity, then integrating the estimated velocity to get an estimated position. To find an object's orientation, we can use the angular rates taken from the IMU's gyroscope—integrating them using quaternions.

IMU Dead Reckoning is useful when GPS data is unavailable. However, please take into account the gradual accumulation of error, which we call "drift".
