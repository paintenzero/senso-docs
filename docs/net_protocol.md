# Network protocol for communicating with Senso Devices Driver

Senso Data Provider listens to TCP port specified in either the UI or command line argument. Once the client connects to that port it starts receiving all devices poses connected to the computer. Different devices data are joined using internal UDP communication between different Senso Data Provider instances.

Every JSON packet is delimited by newline sign ("\n", ASCII=10).

# Glove pose

A pose of the Senso glove.  

`name` and `src` stand for device's BLE name and MAC address respectively. `type` field inside `data` dictionary is used to determine if it is a pose for right hand (rh) or left hand (lh).  
All rotations are given in quaternions in format: `[w, x, y, z]`. Note that axis of the IMU could not coincide with axis used in your game engine so it is up to you to convert the quaternion.
X, Y, Z on sensor corresponds to Pitch, Roll and Yaw respectively.

![Pitch Roll Yaw](img/roll_pitch_yaw.png)

Fingers contain 3 positional units:
* quaternion - current rotation of the sensor on finger
* position - approximation of finger tip location. It is assumed that finger length is 1
* angles - array of two rotation components: first is yaw of the finger while the second stands for pitch. Roll is not used as usually human cannot rotate finger in that axis.

Yaw of the finger rotation can be simply applied to metacarpophalangeal joint of the finger. At the same time a pitch divided by 3 should be applied to all joints of the finger.

```
{
  "src": "Device MAC address",
  "name": "Name of the device",
  "type": "position",
  "data": {
    "type": "rh/lh",
    "palm": {
      "pos": [float, float, float],
      "quat": [float, float, float, float]
    },
    "wrist": {
      "quat": [float, float, float, float]
    },
    "fingers": [
      /* Thumb finger: */
      {
        "pos": [float, float, float],
        "quat": [float, float, float, float],
        "ang": [float, float]
      },
      /* Index finger: */
      {
        "pos": [float, float, float],
        "quat": [float, float, float, float],
        "ang": [float, float]
      },
      /* Middle finger: */
      {
        "pos": [float, float, float],
        "quat": [float, float, float, float],
        "ang": [float, float]
      },
      /* Third finger: */
      {
        "pos": [float, float, float],
        "quat": [float, float, float, float],
        "ang": [float, float]
      },
      /* Little finger: */
      {
        "pos": [float, float, float],
        "quat": [float, float, float, float],
        "ang": [float, float]
      }
    ]
  }
}
```

# Vibration packet

To start vibrating a client should send the following JSON packet to the server:

```
{
  "dst": "Device MAC address",
  "type": "vibration",
  "data": {
    "type": "thumb/index/middle/third/little",
    "dur": <int 0-65536>,
    "str": <int 0-10>
  }
}
```

`dst` field is the MAC address of the device you want to start vibrating on.  
`type` field inside data dictionary should specify which finger you want to start vibration. Instead of finger name there can be integer indices: from 0 for thumb to 4 for little finger.  
`dur` is the duration of the vibration in milliseconds
`str` is the strength of the vibration. Valid values are from 0 to 10.

To stop vibrating before duration expired you want to pass packet with strength set to 0.

# HMD orientation packet

Client should specify orientation of the HMD. This is a rotation to better adjust hand orientation in the global space.

```
{
  "type": "orientation",
  "data": {
    "rot": [float, float, float]
  }
}
```

Rotation is given in radians around axes pitch, roll and yaw respectively.
