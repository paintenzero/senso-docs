# Network protocol for communicating with Senso Devices Driver

## Glove pose

A pose of the Senso Device. Quaternion format: `[x, y, z, w]`

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
        "quat": [float, float, float, float]
      },
      /* Index finger: */
      {
        "pos": [int, int, int],
        "quat": [float, float, float, float]
      },
      /* Middle finger: */
      {
        "pos": [int, int, int],
        "quat": [float, float, float, float]
      },
      /* Third finger: */
      {
        "pos": [int, int, int],
        "quat": [float, float, float, float]
      },
      /* Little finger: */
      {
        "pos": [int, int, int],
        "quat": [float, float, float, float]
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
