---
date: "2026-06-08T15:39:52+03:00"
title: "Wasa dataset"
---

This is the dataset and its description for "Towards Collecting Large-Scale Vehicular Sensor Data for Open Access" paper.

TODO: link

_B. Kämä, O. Timonen, N. Stafford, J. Lotvonen, O. Kalliokoski, A. Kanerva, P. Rathnayake, S. Gamage, A. Vuorinen, S. Ariram, E. Gilman & E. Peltonen.
Towards Collecting Large-Scale Vehicular Sensor Data for Open Access.
TODO: published where?_

## How the Dataset was Collected

The dataset was collected by driving in urban areas of Oulu, Finland while having sensors installed on the roof of the car. Sensor data was collected and recorded via ROS2 on a Jetson AGX computer. Recorded data includes stereo camera images, thermal camera images, lidar pointcloud, plus the car's CAN data and GPS information.

### Route and environmental specifics

Three separate people drove through the same route in Oulu on a sunny day in June. The city part includes cobblestones, traffic lights, parked vehicles, and pedestrians. The highway route begins with merging onto the highway, and then sustaining 80-100km/h speed.

| City route                                                   | Highway route                                                   |
| ------------------------------------------------------------ | --------------------------------------------------------------- |
| ![city route](./images/city_route.png)                       | ![highway route](./images/highway_route.png)                    |
| [City route link](https://maps.app.goo.gl/3UnANSPNsRBDGEDu9) | [Highway route link](https://maps.app.goo.gl/GxyeRvVVPpaBBWdR6) |

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

/aux/image_color:

![aux_image_color](./images/aux_image_color_example.png)

| /left/image_rect                                         | /right/image_rect                                          |
| -------------------------------------------------------- | ---------------------------------------------------------- |
| ![left_image_rect](./images/left_image_rect_example.png) | ![right_image_rect](./images/right_image_rect_example.png) |

/left/depth:

![left_depth](./images/left_depth_example.png)

/left/cost:

![left_cost](./images/left_cost_example.png)

| /flir_0/image_raw                               | /flir_1/image_raw                                |
| ----------------------------------------------- | ------------------------------------------------ |
| ![left_image_rect](./images/flir_0_example.png) | ![right_image_rect](./images/flir_1_example.png) |

/lidar_points:

![lidar_points](./images/lidar_example.png)

CAN bus .json:

```json
    {
        "timestamp": 1780480412.007036,
        "frame_id": "0x1D3",
        "message": "PCM_CRUISE_2",
        "signal": "CHECKSUM",
        "value": 4
    },
    {
        "timestamp": 1780480412.007036,
        "frame_id": "0xB4",
        "message": "SPEED",
        "signal": "ENCODER",
        "value": 45
    },
    {
        "timestamp": 1780480412.007036,
        "frame_id": "0xB4",
        "message": "SPEED",
        "signal": "SPEED",
        "value": 28.18
    },
```

GPS .json:

```json
    {
        "timestamp": 1780480412.039233,
        "frame_id": "0x601",
        "message": "BMC_MagneticField",
        "signal": "MagneticField_X",
        "value": -71.7
    },
    {
        "timestamp": 1780480412.039233,
        "frame_id": "0x601",
        "message": "BMC_MagneticField",
        "signal": "MagneticField_Y",
        "value": 41.699999999999996
    },
    {
        "timestamp": 1780480412.039233,
        "frame_id": "0x601",
        "message": "BMC_MagneticField",
        "signal": "MagneticField_Z",
        "value": -66.3
    },
```

Timestamps of all files are included in the file name in [Unix time format](https://en.wikipedia.org/wiki/Unix_time).

## Download methods

city1
city2
city3
highway1
highway2
highway3

For more options, check [Allas Docs](https://docs.csc.fi/data/Allas/accessing_allas/#commandline-tools).
