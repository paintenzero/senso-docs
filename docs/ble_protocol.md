# Senso BLE protocol

Common UUID for all Senso characteristics: a8bd_XXXX_-0c74-471e-bd1c-9cec99925f02

### Receiving senso glove IMU data

Characteristic UUID: 0x0001  
Full UUID: a8bd0001-0c74-471e-bd1c-9cec99925f02

GATT central should subscribe to this characteristic to receive positional data from the device. The format of the data depends on mode written to the corresponding characteristic. All data is sent in little-endian.

First 2 bytes are always an unsigned sequence that is incremented each time the data is sent.
For mode 1 and 3 first 105 bytes are the same:  
7 times  
3 bytes for each axis of gyroscope + 2 bytes for each axis of accelerometer  

For *mode 3* follows 6 bytes for magnetometer (2 bytes for each axis) and 1 byte for temperature.

Mode 1 and 3 ends with 2 bytes of gyroscope counter - how many measurements was made since the device was connected.

### Sensors mode

Characteristic UUID: 0x0002  
Full UUID: a8bd0001-0c74-471e-bd1c-9cec99925f02

Sets data format sent in characteristic 0x0001

### Sensors datarate

Characteristic UUID: 0x0003  
Full UUID: a8bd0001-0c74-471e-bd1c-9cec99925f02
Default value: 50  

1 byte. Sets how fast the device should notify characteristic 1 with new data.

### Sensors additional data

Characteristic UUID: 0x0004  
Full UUID: a8bd0001-0c74-471e-bd1c-9cec99925f02

Active only in mode 1. The characteristic is notified with additional sensors data: 2 bytes for each axis and 2 byte for temperature.

### Vibrate

Characteristic UUID: 0x0101  
Full UUID: a8bd0101-0c74-471e-bd1c-9cec99925f02

GATT central should send 1 byte 0xFF every 10 seconds to this characteristic to keep the device connected.

To start vibrating GATT central should send 6 bytes in following format:  
1 byte for duration in 5 milliseconds.  
5 byte patterns. 1 byte for each actuator from thumb to little. Every bit in the pattern means if the actuator should be on (1) or off (0) in the given piece of time (measured in 5 ms).

### Managing persistent settings

Characteristic UUID: 0x0201  
Full UUID: a8bd0201-0c74-471e-bd1c-9cec99925f02

The first byte is control byte. It consists of two parts: higher 4 bits is used for commands:

- 0000 is used to communicate with non-volatile storage
- 0001 is used to get or change device name
- 0010 is used to get firmware version

In case of communication with NV storage (command _0000_) reading and writing happens in blocks of 12 bytes. Lower 4 bits is used for address of 12-bytes memory block. There are **14 blocks (168 bytes)** available.

In case of writing to the characteristic only the first byte (command), it switches the context so one can read the value stored at location of interest.

For example, to store new name of the board, one need to write packet:
```
0x10 <5 bytes ascii>
```

To get firmware version, write 1 byte `0x20` to the characteristic  and then read it back.
