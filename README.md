# Researchlab Autonomous Shipping Delft - Internal standards

<p align="center">
  <img src="https://user-images.githubusercontent.com/5917472/225585663-8054c647-83d7-4b90-abb3-9ec3994ab30f.png" />
</p>

Interfaces described in this document are prone to change. 

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

Old namespace convention:
| Topicname                                  | Description             | Messagetype        | Default unit(s)                                        |
|-------------------------------------------|-------------------------|--------------------|--------------------------------------------------------|
| /vesselID/u_ref  | Reference/desired Actuator state  | std_msgs/Float32MultiArray* | shaft velocities: Rpm       |
| /vesselID/u_est | Measured actuator state | std_msgs/Float32MultiArray  | identical to /vesselID/u_ref |
| OptiRAS/vesselID | Estimated pose          | std_msgs/pose       | meters,quaternions   |
| /vesselID/geopos_est | Estimated/measured global position | sensor_msgs/NavSatFix      | degrees (latitude, longitude), meters (altitude)   |
| /vesselID/heading_est | Estimated/measured yaw | std_msgs/Float32 | radians (clockwise from above w.r.t. north) |
| /vesselID/heading_ref | Reference/desired yaw | std_msgs/Float32 | radians (clockwise from above w.r.t. north) |

Updated namespaces
| Topicname                                 | Description             | Messagetype        | Default unit(s)                                        |
|-------------------------------------------|-------------------------|--------------------|--------------------------------------------------------|
| /vesselID/reference/actuation  |  Reference/desired Actuator state     | std_msgs/Float32MultiArray* | shaft velocities: Rpm    |
| /vesselID/state/actuation | Measured actuator state | std_msgs/Float32MultiArray  | identical to /vesselID/u_ref |
| /vesselID/state/poseLocal | Estimated/measured pose w.r.t. local coordinate system    | stdmsgs/pose       | meters,quaternions    |
| /vesselID/state/posGlobal | Estimated/measured global position | sensor_msgs/NavSatFix      | degrees (latitude, longitude), meters (altitude)   |
| /vesselID/reference/posGlobal | Reference/desired global position | sensor_msgs/NavSatFix      | degrees (latitude, longitude), meters (altitude)   |
| /vesselID/state/heading | Estimated/measured yaw | std_msgs/Float32 | radians (clockwise from above w.r.t. north) |
| /vesselID/reference/heading | Reference/desired yaw | std_msgs/Float32 | radians (clockwise from above w.r.t. north) |

Actuation of the vessels are defined as follows:


| Vessel Type                                 | Actuation vector interpretation      | Unit |
|-------------------------------------------|-------------------------|--------------------|
| Delfia-1* | $[propvel_{1},propvel\_{2},propangle\_{1},propangle\_{2}]$| $rps,rps,radians, radians$|
| Tito Neri | $[propvel_{aft,ps},propvel\_{aft,sb},proppower\_{bow},propangle\_{aft,ps},propangle\_{aft,sb}]$ | $[rpm,rpm,normalized,radians,radians]$ |
| Grey Seabax | $[]$ |  $[]$ |
