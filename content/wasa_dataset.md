---
date: '2026-06-08T15:39:52+03:00'
title: 'Dataset'
---

# Towards Collecting Large-Scale Vehicular Sensor Data for Open Access Dataset

This is the dataset and its description for "Towards Collecting Large-Scale Vehicular Sensor Data for Open Access" paper.

TODO: link

_B. Kämä, O. Timonen, N. Stafford, J. Lotvonen, O. Kalliokoski, A. Kanerva, P. Rathnayake, S. Gamage, A. Vuorinen, S. Ariram, E. Gilman & E. Peltonen.
Towards Collecting Large-Scale Vehicular Sensor Data for Open Access.
TODO: published where?_

## How the Dataset was Collected

The dataset was collected by driving in urban areas of Oulu, Finland while having sensors installed on the roof of the car. Sensor data was collected and recorded via ROS2 on a Jetson AGX computer. Recorded data includes stereo camera images, thermal camera images, lidar pointcloud, plus the car's CAN data and GPS information.

### Route and environmental specifics

Three separate people drove through the same route in Oulu on a sunny day in June. The city part includes cobblestones, traffic lights, parked vehicles, and pedestrians. The highway route begins with merging onto the highway, and then sustaining 80-100km/h speed.

![city route](/images/city_route.png)

[City route](https://maps.app.goo.gl/3UnANSPNsRBDGEDu9)

![highway route](/images/highway_route.png)
[Highway route](https://maps.app.goo.gl/GxyeRvVVPpaBBWdR6)

### Computer and software setup

The computer inside the car is a Jetson AGX Orin Developer Kit on Ubuntu 22.04 running ROS2 nodes in docker containers. The nodes include sensor drivers, and a recorder to write the messages on an SSD in the form of [.mcap](https://mcap.dev/) files. [The source code for our setup is publicly available on github](https://github.com/M3S-Kuura/ros2-device-control).

### Car and sensor setup

The car used was a Toyota RAV4 Hybrid 2019. Its CAN bus is read and translated into known signals using opendbc's DBC file. GPS signal is collected via PCAN-GPS.

Thermal cameras: [FLIR ADK 2.0 LWIR](https://www.acalbfi.com/technologies/imaging/infrared-imaging/infrared-thermal-cores-and-lenses/adktm-20-lwir-thermal-imaging-core/)

Stereo camera: [Carnegie Robotics Multisense S27](https://www.carnegierobotics.com/multisense-s27)

Lidar: [Hesai OT128 Lidar](https://www.hesaitech.com/product/ot128/)

Recording settings for Lidar:

- Low resolution
- Single-point first return
- 600rpm

![image of the roof](./images/sensors_on_roof.jpg)

## Data format

The dataset has people's faces and vehicles' license plates blurred to respect people's privacy. Hence the dataset does not contain the original .mcap files which would include unblurred images.

CAN, gps .json

| Data source              | Description                                  | Original format             | Dataset format       |
| ------------------------ | -------------------------------------------- | --------------------------- | -------------------- |
| /lidar_points ROS2 topic | LiDAR                                        | PointCloud2 (230400 points) | .laz                 |
| /image_raw ROS2 topic    | Thermal cameras                              | mono16                      | 8-bit colormap .png  |
| /aux/image_color         | Multisense's middle camera                   | bgr8                        | 8-bit rgb .png       |
| /left/image_rect         | Multisense's left camera                     | mono8                       | 8-bit grayscale .png |
| /right/image_rect        | Multisense's right camera                    | mono8                       | 8-bit grayscale .png |
| /left/depth              | Multisense's estimated depth distance        | 32FC1                       | 8-bit colormap .png  |
| /left/cost               | Multisense's confidence value of /left/depth | mono8                       | 8-bit grayscale .png |
| Car CAN bus              | Car sensor messages                          | Hexadecimal bytes           | Human-readable .json |
| PCAN-GPS                 | GPS sensor messages                          | Hexadecimal bytes           | Human-readable .json |

All unknown CAN & GPS signals have been left out of the dataset, and known signals have been converted to human-readable form.

## Data examples

images

json files

## Download links

city1
city2
city3
highway1
highway2
highway3
