## Glove position

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
      },
    ]
  }
}
```
