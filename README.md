# Researchlab Autonomous Shipping Delft - Internal standards
<p align="center">
  <img src="https://user-images.githubusercontent.com/5917472/225625088-a7cb7d74-f594-4aed-bfc7-555cc0f2d2e4.png" />
</p>


These guidelines are aimed to help people get access to RAS systems and encourage alignment of interfaces between projects/modules. Anyone can choose freely to ignore these guidelines as they see fit, but it is encouraged to follow this standard to enhance component compatibility.

For descriptions of common control setups, [see here](baseStackOverview.md)
<p align="center">
  <img src="https://user-images.githubusercontent.com/5917472/225585663-8054c647-83d7-4b90-abb3-9ec3994ab30f.png"/>
</p>

## Namespaces 
The table below shows identifiers are used for our vessels. These names are commonly used when something needs to refer to a specific ship, such as ROS topics. 

| Model      | Description          | Identifier  |
|------------|----------------------|-------------|
| TitoNeri   | Light-blue Tito Neri | RAS\_TN\_LB |
| TitoNeri   | Dark-blue Tito Neri  | RAS\_TN\_DB |
| TitoNeri   | Red Tito Neri        | RAS\_TN\_RE |
| TitoNeri   | Yellow Tito Neri     | RAS\_TN\_YE |
| TitoNeri   | Purple Tito Neri     | RAS\_TN\_PU |
| TitoNeri   | Green Tito Neri      | RAS\_TN\_GR |
| TitoNeri   | Orange Tito Neri     | RAS\_TN\_OR |
| GreySeabax | The Grey-seabax      | RAS\_GS     |
| Delfia-1*  | Delfia 1             | RAS\_DF\_1  |
| Delfia-1*  | Delfia 2             | RAS\_DF\_2  |
| Delfia-1*  | Delfia 3             | RAS\_DF\_3  |
| Delfia-1*  | Delfia 4             | RAS\_DF\_4  |
| Delfia-1*  | Delfia 5             | RAS\_DF\_5  |

## ROS topics & message formats
To have modules easily swappable and connectable, the following default messagetypes and namespacing are suggested. 'vesselID' refers to the Identifier of a particular vessel as described in the previous section.

Old namespace convention: (pre 2023)
| Topicname                                  | Description             | Messagetype        | Default unit(s)                                        |
|-------------------------------------------|-------------------------|--------------------|--------------------------------------------------------|
| /vesselID/u_ref  | Reference/desired Actuator state  | std_msgs/Float32MultiArray* | (see [below](#details-on-particular-topics) )   |
| /vesselID/u_est | Measured actuator state | std_msgs/Float32MultiArray  | identical to /vesselID/u_ref |
| OptiRAS/vesselID | Estimated pose          | std_msgs/pose       | meters,quaternions   |
| /vesselID/geopos_est | Estimated/measured global position | sensor_msgs/NavSatFix      | degrees (latitude, longitude), meters (altitude)   |
| /vesselID/heading_est | Estimated/measured yaw | std_msgs/Float32 | radians (clockwise from above w.r.t. north) |
| /vesselID/heading_ref | Reference/desired yaw | std_msgs/Float32 | radians (clockwise from above w.r.t. north) |
| /vesselID/telemetry  | microcontroller state / telemetry  | std_msgs/Float32MultiArray* | various (see [below](#details-on-particular-topics) )  |

Updated namespaces (we will migrate tho this throughout 2023)
| Topicname                                 | Description             | Messagetype        | Default unit(s)                                        |
|-------------------------------------------|-------------------------|--------------------|--------------------------------------------------------|
| /vesselID/reference/actuation  |  Reference/desired Actuator state     | std_msgs/Float32MultiArray* | (see [below](#details-on-particular-topics) )  |
| /vesselID/reference/actuation_prio  |  Override actuation reference, commonly for emergency | std_msgs/Float32MultiArray* | (see [below](#details-on-particular-topics) )   |
| /vesselID/state/actuation | Measured actuator state | std_msgs/Float32MultiArray  | identical to /vesselID/u_ref |
| /vesselID/state/poseLocal | Estimated/measured pose w.r.t. local coordinate system    | stdmsgs/pose       | meters,quaternions    |
| /vesselID/state/posGlobal | Estimated/measured global position | sensor_msgs/NavSatFix      | degrees (latitude, longitude), meters (altitude)   |
| /vesselID/reference/posGlobal | Reference/desired global position | sensor_msgs/NavSatFix      | degrees (latitude, longitude), meters (altitude)   |
| /vesselID/state/heading | Estimated/measured yaw | std_msgs/Float32 | radians (clockwise from above w.r.t. north) |
| /vesselID/reference/heading | Reference/desired yaw | std_msgs/Float32 | radians (clockwise from above w.r.t. north) |

### Details on particular topics
Delfia-1* actuation array is as follows:
| Index | Content                  | Unit    |
|-------|--------------------------|---------|
| 0     | Rear propeller velocity  | RPS     |
| 1     | Front propeller velocity | RPS     |
| 2     | Angle rear azimuth       | Radians |
| 3     | Angle front azimuth      | Radians |
<br>


For the Tito Neri the actuation array is defined as 

| Index | Content                              | Unit                             |
|-------|--------------------------------------|----------------------------------|
| 0     | Aft portside propeller velocity      | RPM                              |
| 1     | Aft starboardside propeller velocity | RPM                              |
| 2     | Bow thruster velocity                | Normalized between -1.0 and +1.0 |
| 3     | Aft portside azimuth angle           | Radians                          |
| 4     | Aft portside azimuth angle           | Radians                          |

<br>
For the Tito Neri the telemetry array is defined as:

| Index | Content                   | Unit                                 |
|-------|---------------------------|--------------------------------------|
| 0     | dcmotor\_P\_reference     | RPM                                  |
| 1     | dcmotor\_SB\_reference    | RPM                                  |
| 2     | output\_BOW               | float, normalized on range -1.0:1.0) |
| 3     | reference\_Angle\_P       | deg                                  |
| 4     | reference\_Angle\_SB      | deg                                  |
| 5     | dcmotor\_P\_estimated     | RPM                                  |
| 6     | dcmotor\_SB\_estimated    | RPM                                  |
| 7     | dcmotorPID\_P\_Output     | normalized on 8 bit reslution 0-255  |
| 8     | dcmotorPID\_SB\_Output    | normalized on 8 bit reslution 0-255  |
| 9    | arduinoInternalClock      | ms                                   |
| 10    | IMU\_accell\_x            | m/s/s                                |
| 11    | IMU\_accell\_y            | m/s/s                                |
| 12    | IMU\_accell\_z            | m/s/s                                |
| 13    | IMU\_gyro\_x              | rad/s                                |
| 14    | IMU\_gyro\_y              | rad/s                                |
| 15    | IMU\_gyro\_z              | rad/s                                |
| 16    | IMU\_magneto\_x           | uT                                   |
| 17    | IMU\_magneto\_y           | uT                                   |
| 18    | IMU\_magneto\_z           | uT                                   |
| 19    | motor\_ESC\_SignalOut\_P  | 1200-1800Hz                          |
| 20    | motor\_ESC\_SignalOut\_SB | 1200-1800Hz                          |

## Coordinate systems
Various basins are equipped with systems that can find and stream state of rigid bodies, such as the Optitrack systems. This section explains in what coordinate system you can expect data to be streamed on ROS with our default system. 

### Tiny lab tank
The coordinate system of the optitrack system on the tiny lab tank is defined as follows:

<p align="center">
  <img src="https://user-images.githubusercontent.com/5917472/225598107-5fb8690f-e711-4db0-9817-42325f938185.jpg" />
</p>

Note that Z does not point down (as in line with the marine standard NED definition). If this is desired, coordinate system transformation can be done by the user.

### Flume tank
![image](https://user-images.githubusercontent.com/5917472/225601576-15fd108c-e240-41bf-ba62-5503a229fb05.png)

Arrows the the figure above denote positive values. Note that Z does not point down (as in line with the marine standard NED definition). If this is desired, coordinate system transformation can be done by the user.

### Small towing tank
