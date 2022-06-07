# Mech-Eye ROS2 Interface

This repository contains the official ROS2 interface for Mech-Eye camera.

## Installation

### Dependencies

|   Package    |    Version    |
| :----------: | :-----------: |
|    OpenCV    |     >= 3      |
|     PCL      | >= 1.7 or 1.8 |
| Mech-Eye SDK |     1.5.2     |

### Install Mech-Eye SDK

Download and install MechEyeApi_1.5.2 compatible with Ubuntu from this [link](https://www.mech-mind.com/download/camera-sdk.html).

### Install ROS2 Galactic

This interface is tested on Ubuntu 20.04 with ROS2 Galactic. You can install ROS2 Galactic following this [page](https://docs.ros.org/en/galactic/Installation/Ubuntu-Install-Debians.html)

### Install OpenCV & PCL

```bash
sudo apt install libopencv-dev
sudo apt install ros-galactic-cv-bridge
sudo apt install libpcl-dev
sudo apt install ros-galactic-pcl-conversions
```

### Install the Interace

```bash
mkdir -p ~/ros_ws/src && cd ~/ros_ws/src
git clone https://github.com/MechMindRobotics/mecheye_ros2_interface.git
cd ~/ros_ws && colcon build
```

## Brief intro to the interface

- Interface functions can be found in the documentation inside [Mech-Eye SDK](https://www.mech-mind.com/download/camera-sdk.html).
- Change config in `~/ros_ws/src/mecheye_ros2_interface/launch/start_camera.py`
  - save_file: `True` to enable save file, otherwise keep it as `False`
  - camera_ip: change to your camera ip address here (also remember to comment and uncomment the lines in `MechMindCamera.cpp` to connect to a specific camera)
  - at the moment, image save path can only be changed in source code `/mecheye_ros2_interface/src/MechMindCamera.cpp`.
  - remember to `colcon build` again after changing `*.cpp`.
- Source the build workspace and use ros2 launch

  ```bash
  source ~/ros_ws/install/setup.bash
  ros2 launch ~/ros_ws/src/mecheye_ros2_interface/launch/start_camera.py 
  ```

  - Or use ros2 run

    ```bash
    source ~/ros_ws/install/setup.bash
    ros2 run mecheye_ros_interface start
    ```

- Open a new terminal, source the workspace and call services

  ```bash
  source ~/ros_ws/install/setup.bash
  ros2 service call [/service_name] [mecheye_ros_interface/srv/ServiceName] "{parameter_name: parameter_value}"
  ```

  ```bash
  # Examples
  ros2 service call /set_laser_settings mecheye_ros_interface/srv/SetLaserSettings "{fringe_coding_mode: 'Fast', frame_range_start: 0, frame_range_end: 50, frame_partition_count: 1, power_level: 20}"
  ros2 service call /set_3d_exposure mecheye_ros_interface/srv/Set3DExposure "{sequence: [3.2, 4.0]}"
  ros2 service call /set_cloud_outlier_filter_mode mecheye_ros_interface/srv/SetCloudOutlierFilterMode "{value: 'Off'}"
  ```

## Topics

### /mechmind/camera_info

Camera calibration and metadata.

### /mechmind/color_image

Color image encoded as "bgr8".

### /mechmind/depth_image

Depth image encoded as 32-bit float.

### /mechmind/point_cloud

Point cloud data.

### /mechmind/color_point_cloud

Color point cloud data.

## Services

### [add_user_set](https://github.com/MechMindRobotics/mecheye_ros2_interface/blob/master/srv/AddUserSet.srv)

Invoke this service to add a user set.

This service has one parameter:

`value` (string): User set name to add.

### [capture_color_map](https://github.com/MechMindRobotics/mecheye_ros2_interface/blob/master/srv/CaptureColorMap.srv)

Invoke this service to capture color map once.

### [capture_color_point_cloud](https://github.com/MechMindRobotics/mecheye_ros2_interface/blob/master/srv/CaptureColorPointCloud.srv)

Invoke this service to capture color point cloud once.

### [capture_depth_map](https://github.com/MechMindRobotics/mecheye_ros2_interface/blob/master/srv/CaptureDepthMap.srv)

Invoke this service to capture color depth map once.

### [capture_point_cloud](https://github.com/MechMindRobotics/mecheye_ros2_interface/blob/master/srv/CapturePointCloud.srv)

Invoke this service to capture point cloud once.

### [delete_user_set](https://github.com/MechMindRobotics/mecheye_ros2_interface/blob/master/srv/DeleteUserSet.srv)

Invoke this service to delete a specified user set.

This service has one parameter:

`value` (string): User set name to delete.

### [device_info](https://github.com/MechMindRobotics/mecheye_ros2_interface/blob/master/srv/DeviceInfo.srv)

Invoke this service to get the current connected device information.

### [get_2d_expected_gray_value](https://github.com/MechMindRobotics/mecheye_ros2_interface/blob/master/srv/Get2DExpectedGrayValue.srv)

Invoke this service to get the expected image gray value.  
Only take effect when 2D exposure mode is `Auto`.  
A smaller value can decrease the brightness of the image, while a larger value can generate a brighter image.

### [get_2d_exposure_mode](https://github.com/MechMindRobotics/mecheye_ros2_interface/blob/master/srv/Get2DExposureMode.srv)

Invoke this service to get current 2D exposure mode.  

### [get_2d_exposure_sequence](https://github.com/MechMindRobotics/mecheye_ros2_interface/blob/master/srv/Get2DExposureSequence.srv)

Invoke this service to get current 2D HDR exposure sequence.  
Only take effect when 2D exposure mode is `HDR`.

### [get_2d_exposure_time](https://github.com/MechMindRobotics/mecheye_ros2_interface/blob/master/srv/Get2DExposureTime.srv)

Invoke this service to get current 2D exposure time.  
Only take effect when 2D exposure mode is `Timed`.

### [get_2d_roi](https://github.com/MechMindRobotics/mecheye_ros2_interface/blob/master/srv/Get2DROI.srv)

Invoke this service to get current ROI when scanning 2D images.  
Only take effect when 2D exposure mode is `Auto` or `HDR`.

### [get_2d_sharpen_factor](https://github.com/MechMindRobotics/mecheye_ros2_interface/blob/master/srv/Get2DSharpenFactor.srv)

Invoke this service to get current image sharpen factor.

### [get_2d_tone_mapping](https://github.com/MechMindRobotics/mecheye_ros2_interface/blob/master/srv/Get2DToneMappingEnable.srv)

Invoke this service to get whether or not is tone mapping enabled.  
Tone mapping uses gray level transformation algorithm to make the image look more natural.

### [get_3d_exposure](https://github.com/MechMindRobotics/mecheye_ros2_interface/blob/master/srv/Get3DExposure.srv)

Invoke this service to get current 3D exposure sequence.

### [get_3d_gain](https://github.com/MechMindRobotics/mecheye_ros2_interface/blob/master/srv/Get3DGain.srv)

Invoke this service to get current camera's gain value during scanning 3D images.  
Gain is an electronic amplification of the image signal. Large gain value is needed only when scanning extremely dark objects.

### [get_3d_roi](https://github.com/MechMindRobotics/mecheye_ros2_interface/blob/master/srv/Get3DROI.srv)

Invoke this service to get current depth map's ROI.

### [get_all_user_sets](https://github.com/MechMindRobotics/mecheye_ros2_interface/blob/master/srv/GetAllUserSets.srv)

Invoke this service to get all available user sets.

### [get_cloud_outlier_filter_mode](https://github.com/MechMindRobotics/mecheye_ros2_interface/blob/master/srv/GetCloudOutlierFilterMode.srv)

Invoke this service to get current cloud outler filter mode.

### [get_cloud_smooth_mode](https://github.com/MechMindRobotics/mecheye_ros2_interface/blob/master/srv/GetCloudSmoothMode.srv)

Invoke this service to get current cloud smooth filter mode.

### [get_current_user_set](https://github.com/MechMindRobotics/mecheye_ros2_interface/blob/master/srv/GetCurrentUserSet.srv)

Invoke this service to get the current user set name.

### [get_depth_range](https://github.com/MechMindRobotics/mecheye_ros2_interface/blob/master/srv/GetDepthRange.srv)

Invoke this service to get the current depth image's valid range along Z-axis in the camera coordinate system.

### [get_fringe_contrast_threshold](https://github.com/MechMindRobotics/mecheye_ros2_interface/blob/master/srv/GetFringeContrastThreshold.srv)

Invoke this service to get the current signal contrast threshold for effective pixels. Pixels with contrast less than this threshold will be ignored.

### [get_fringe_min_threshold](https://github.com/MechMindRobotics/mecheye_ros2_interface/blob/master/srv/GetFringeMinThreshold.srv)

Invoke this service to get the current signal minimum threshold for effective pixels. Pixels with intensity less than this threshold will be ignored.

### [get_laser_settings](https://github.com/MechMindRobotics/mecheye_ros2_interface/blob/master/srv/GetLaserSettings.srv)

Invoke this service to get the current laser settings.

### [set_2d_expected_gray_value](https://github.com/MechMindRobotics/mecheye_ros2_interface/blob/master/srv/Set2DExpectedGrayValue.srv)

Invoke this service to set the expected image gray value.  
Only take effect when 2D exposure mode is `Auto`.  
A smaller value can decrease the brightness of the image, while a larger value can generate a brighter image.

This service has one parameter:

`value` (int32): Expected image gray value to set. Min:0, Max: 255.

### [set_2d_exposure_mode](https://github.com/MechMindRobotics/mecheye_ros2_interface/blob/master/srv/Set2DExposureMode.srv)

Invoke this service to set current 2D exposure mode.  

This service has one parameter:

`value` (string): 2D exposure mode to set. Options include 'Timed', 'Auto', 'HDR', and 'Flash'.

### [set_2d_exposure_sequence](https://github.com/MechMindRobotics/mecheye_ros2_interface/blob/master/srv/Set2DExposureSequence.srv)

Invoke this service to set current 2D HDR exposure sequence.  
Only take effect when 2D exposure mode is `HDR`.

This service has one parameter:

`sequence` (float64[]): 2D exposure sequence to set. Min: 0.1, Max: 999. Min Size: 1, Max Size: 5.

### [set_2d_exposure_time](https://github.com/MechMindRobotics/mecheye_ros2_interface/blob/master/srv/Set2DExposureTime.srv)

Invoke this service to set current 2D exposure time.  
Only take effect when 2D exposure mode is `Timed`.

This service has one parameter:

`value` (float64): 2D exposure time to set. Min: 0.1, Max: 999.

### [set_2d_roi](https://github.com/MechMindRobotics/mecheye_ros2_interface/blob/master/srv/Set2DROI.srv)

Invoke this service to set current ROI when scanning 2D images.  
Only take effect when 2D exposure mode is `Auto` or `HDR`.

This service has four parameters:

`x` (uint32): The column coordinates of the upper left point of the region of interest.  
`y` (uint32): The row coordinates of the upper left point of the region of interest.  
`width` (uint32): The width of the region of interest.  
`height` (uint32): The height of the region of interest.

### [set_2d_sharpen_factor](https://github.com/MechMindRobotics/mecheye_ros2_interface/blob/master/srv/Set2DSharpenFactor.srv)

Invoke this service to set current image sharpen factor.

This service has one parameter:

`value` (float64): The sharpen factor to set. Min: 0, Max: 5.

### [set_2d_tone_mapping](https://github.com/MechMindRobotics/mecheye_ros2_interface/blob/master/srv/Set2DToneMappingEnable.srv)

Invoke this service to set whether or not is tone mapping enabled.  
Tone mapping uses gray level transformation algorithm to make the image look more natural.

This service has one parameter:

`value` (bool): Whether or not to enable tone mapping.

### [set_3d_exposure](https://github.com/MechMindRobotics/mecheye_ros2_interface/blob/master/srv/Set3DExposure.srv)

Invoke this service to set current 3D exposure sequence.

This service has one parameter:

`sequence` (float64[]): Exposure sequence to set. Min: 0.1, Max: 99. Min Size: 1, Max Size: 3.

### [set_3d_gain](https://github.com/MechMindRobotics/mecheye_ros2_interface/blob/master/srv/Set3DGain.srv)

Invoke this service to set current camera's gain value during scanning 3D images.  
Gain is an electronic amplification of the image signal. Large gain value is needed only when scanning extremely dark objects.

This service has one parameter:

`value` (float64): Gain value to set. Min: 0, Max: 16.

### [set_3d_roi](https://github.com/MechMindRobotics/mecheye_ros2_interface/blob/master/srv/Set3DROI.srv)

Invoke this service to set current depth map's ROI.

This service has four parameters:

`x` (uint32): The column coordinates of the upper left point of the region of interest.  
`y` (uint32): The row coordinates of the upper left point of the region of interest.  
`width` (uint32): The width of the region of interest.  
`height` (uint32): The height of the region of interest.

### [set_cloud_outlier_filter_mode](https://github.com/MechMindRobotics/mecheye_ros2_interface/blob/master/srv/SetCloudOutlierFilterMode.srv)

Invoke this service to set current cloud outler filter mode.

This service has one parameter:

`value` (string): Cloud outlier filter mode to set. Options include 'Off', 'Normal', and 'Weak'.

### [set_cloud_smooth_mode](https://github.com/MechMindRobotics/mecheye_ros2_interface/blob/master/srv/SetCloudSmoothMode.srv)

Invoke this service to set current cloud smooth filter mode.

This service has one parameter:

`value` (string): Cloud smooth mode to set. Options include 'Off', 'Normal', 'Weak', and 'Strong'.

### [set_current_user_set](https://github.com/MechMindRobotics/mecheye_ros2_interface/blob/master/srv/SetCurrentUserSet.srv)

Invoke this service to set the current user set name.

This service has one parameter:

`value` (string): User set name to set.

### [set_depth_range](https://github.com/MechMindRobotics/mecheye_ros2_interface/blob/master/srv/SetDepthRange.srv)

Invoke this service to set the current depth image's valid range along Z-axis in the camera coordinate system.

This service has two parameters:

`lower` (int32): The lower limit of the roi on the z value of the depth map in the camera coordinate system.  
`upper` (int32): The upper limit of the roi on the z value of the depth map in the camera coordinate system.

### [set_fringe_contrast_threshold](https://github.com/MechMindRobotics/mecheye_ros2_interface/blob/master/srv/SetFringeContrastThreshold.srv)

Invoke this service to set the current signal contrast threshold for effective pixels. Pixels with contrast less than this threshold will be ignored.

This service has one parameter:

`value` (int32): Signal contrast threshold to set. Min: 0, Max: 100.

### [set_fringe_min_threshold](https://github.com/MechMindRobotics/mecheye_ros2_interface/blob/master/srv/SetFringeMinThreshold.srv)

Invoke this service to set the current signal minimum threshold for effective pixels. Pixels with intensity less than this threshold will be ignored.

This service has one parameter:

`value` (int32): Signal minimum threshold to set. Min: 0, Max: 100.

### [set_laser_settings](https://github.com/MechMindRobotics/mecheye_ros2_interface/blob/master/srv/SetLaserSettings.srv)

Invoke this service to set the current laser settings.

This service has five parameters:

`fringe_coding_mode` (string): Laser fringe coding mode to set. Options include 'Fast', and 'High'.  
`frame_range_start` (int32): The laser scan field of view start position to set. Min: 0, Max: 100.  
frameRangeEnd - frameRangeStart >= 25  
`frame_range_end` (int32): The laser scan field of view end position to set. Min: 0, Max: 100.  
frameRangeEnd - frameRangeStart >= 25  
`frame_partition_count` (int32): Laser's scan partition number to set. Min: 1, Max: 4.  
`power_level` (int32): Laser's power level to set. Min: 20, Max: 100.
