# Network protocol for communicating with Senso Devices Driver

Once the client connects to the server it starts receiving all device poses connected to the server.

## Glove pose

A pose of the Senso glove.  
Quaternion format: `[w, x, y, z]`  
Angles for finger format: `[yaw, pitch]`

```
{
  "src": "Device MAC address",
  "name": "Name of the device",
  "type": "position",
  "data": {
    "type": "rh/lh",
    "palm": {
      "pos": [int, int, int],
      "quat": [float, float, float, float]
    },
    "wrist": {
      "quat": [float, float, float, float]
    },
    "fingers": [
      /* Thumb finger: */
      {
        "pos": [int, int, int],
        "quat": [float, float, float, float],
        "ang": [float, float]
      },
      /* Index finger: */
      {
        "pos": [int, int, int],
        "quat": [float, float, float, float],
        "ang": [float, float]
      },
      /* Middle finger: */
      {
        "pos": [int, int, int],
        "quat": [float, float, float, float],
        "ang": [float, float]
      },
      /* Third finger: */
      {
        "pos": [int, int, int],
        "quat": [float, float, float, float],
        "ang": [float, float]
      },
      /* Little finger: */
      {
        "pos": [int, int, int],
        "quat": [float, float, float, float],
        "ang": [float, float]
      }
    ]
  }
}
```

## Vibration packets

```
{
  "dst": "Device MAC address",
  "type": "vibration",
  "data": {
    "type": "thumb/index/middle/third/little",
    "dur": <int 0-65536 in milliseconds>,
    "str": <int 0-10>
  }
}
```

Instead of finger type there can be indices: 0 for thumb to 4 for little finger
