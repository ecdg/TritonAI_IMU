# Using an IMU
## Creating an IMU package (on DonkeyCar—has some overlap with the process of creating an IMU package on ROS2)
### A script handling the data from the IMU
The standard DonkeyCar installation includes support for the **MPU-6050** and **MPU-9250** IMU sensors. They have [this script](https://github.com/autorope/donkeycar/blob/main/donkeycar/parts/imu.py) for handling the data from these IMU sensors.

For the **SparkFun 9DoF Razor IMU**, we can use [this script](https://github.com/NikitaB04/razorIMU_9dof/blob/main/imu.py).

For the **BNO086 IMU of the OAK-D Pro camera**, we have this [modified script](https://github.com/rohanmeserve/dsc190_imu_oakd/blob/main/imu_oakd.py) adding support for this specific IMU. These documents [(1)](https://docs.luxonis.com/projects/api/en/latest/components/nodes/imu/#)[(2)](https://docs.luxonis.com/projects/api/en/latest/samples/IMU/imu_accelerometer_gyroscope/#imu-accelerometer-gyroscope) were used as a guide by the DSC folks when creating the modified script.

If you are using a different IMU sensor, then look into its support documents for handling its data and whether or not they provide a script that does this. If there is none, then you can use its documents as a reference as well as other IMU scripts as a guide when creating one.

### To run on DonkeyCar
First, import the IMU script, e.g. imu.py, onto the car folder. Next, modify the complete.py file (it should be in the car folder) so that it imports the IMU object from the script. Then, edit the manage.py file so it imports from the modified complete.py file, which includes an ``add_imu``.

For how it looks:
- The change in complete.py, where the IMU object is imported from (highlighted section)
  ![modified_complete](https://github.com/ecdg/TritonAI_IMU/blob/main/docs/modified_complete_file.png)

- Adding a new part to DonkeyCar in manage.py: import object from file, instantiate object, V.add(object, inputs, outputs)
  ![add_part](https://github.com/ecdg/TritonAI_IMU/blob/main/docs/add_part_manage_file.png)

- If you decide to create a **__separate__** complete.py file, then make sure to change the import path in manage.py (highlighted section)
  ![separate_complete](https://github.com/ecdg/TritonAI_IMU/blob/main/docs/if_separate_complete_file.png)


## Pose estimation using an IMU sensor
Here are implementations of pose estimation using an IMU done in the Triton AI lab. These documents provide more details about these implementations:
1. [IMU & GPS Sensor Fusion](https://github.com/ecdg/TritonAI_IMU/blob/main/docs/imu_gps_fusion.md)
2. [IMU Dead Reckoning](https://github.com/ecdg/TritonAI_IMU/blob/main/docs/imu_dead_reckoning.md)
