# Network protocol for communicating with Senso Devices Driver

Senso Data Provider listens to TCP port specified in either the UI or command line argument. Once the client connects to that port it starts receiving all devices poses connected to the computer. Different devices data are joined using internal UDP communication between different Senso Data Provider instances.

Every JSON packet is delimited by newline sign ("\n", ASCII=10).

# Glove pose

A pose of the Senso glove.  

`name` and `src` stand for device's BLE name and MAC address respectively. `type` field inside `data` dictionary is used to determine if it is a pose for right hand (rh) or left hand (lh).  
All rotations are given in quaternions in format: `[w, x, y, z]`. Note that axis of the IMU doesn't coincide with axis used in your game engine so it is up to you to convert the quaternion.
X, Y, Z on sensor corresponds to Pitch, Roll and Yaw respectively.

![Pitch Roll Yaw](img/roll_pitch_yaw.png)

For palm there are multiple properties. The most important one is `quat` which is a rotation of the palm. `pos` property is a position of the palm in 3d space and it is calculated only if some tracking is enabled (like lighthouse).
Also for you convenient there is speed `spd` and acceleration `acc` vectors that are filtered data from palm accelerometer.

Each finger contain `ang` property which corresponds to rotation components of a finger. First is yaw of the finger while the second stands for pitch. Roll is not used as usually human cannot rotate finger in that axis.

Yaw of the finger rotation can be simply applied to metacarpophalangeal joint of the finger. At the same time a pitch divided by 3 should be applied to all joints of the finger.

In Senso Glove DK2 there are two sensors for thumb finger. First sensor gives us a `quat` value which is a base rotation (metacarpophalangeal joint) of a thumb. `bend` value corresponds to bending of a distal phalanx.

```
{
	"src": "Device MAC address",
	"name": "Name of the device",
	"fullname": "Full name of the device",
	"type": "position",
	"data": {
		"type": "rh/lh",
		"palm": {
			"pos": [x:float, y:float, z:float],
			"quat": [w:float, x:float, y:float, z:float],
			"spd": [x:float, y:float, z:float],
			"acc": [x:float, y:float, z:float]
		},
		"wrist": {
			"quat": [w:float, x:float, y:float, z:float]
		},
		"shoulder": {
			"quat": [w:float, x:float, y:float, z:float]
		},
		"fingers": [{
			"ang": [0.029587, -1.186242],
			"quat": [w:float, x:float, y:float, z:float],
			"bend": 0.000
		}, {
			"ang": [0.015035, -0.344085]
		}, {
			"ang": [0.018008, -0.002090]
		}, {
			"ang": [0.006028, -0.004824]
		}, {
			"ang": [0.007790, 0.160404]
		}],
		"m0": [x:float, y:float, z:float],
		"m1": [x:float, y:float, z:float],
		"m2": [x:float, y:float, z:float],
		"battery": [int],
		"temperature": [int]
	}
}
```

`m0` is raw data from magnetometer of the palm. `m1` is raw data from magnetometer of the wrist. `m2` is raw data from magnetometer of the shoulder (if shoulder is connected to the glove).
`battery` is a percentage of the glove (0-100 values).

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
    "type": "hmd",
    "yaw": float
  }
}
```

Rotation is given in radians around axes pitch, roll and yaw respectively.

# Gesture

```
{
  "src": "Device MAC address",
  "type": "gesture",
  "data": {
    "hand": "rh/lh",
    "name": "name of the gesture",
    "fingers": [0, 1]
  }
}
```

Valid names of the gestures: "pinch_start", "pinch_end"
