# Senso BLE protocol

Common UUID for all Senso characteristics: a8bd_XXXX_-0c74-471e-bd1c-9cec99925f02

### Managing persistent settings

Characteristic UUID: 0x0201  
Full UUID: a8bd0201-0c74-471e-bd1c-9cec99925f02

The first byte is control byte.It consists of two parts: higher 4 bits is used for commands:

- 0000 is used to communicate with non-volatile storage
- 0001 is used to get or change device name
- 0010 is used to get firmware version

In case of communication with NV storage (command _0000_) reading and writing happens in blocks of 12 bytes. Lower 4 bits is used for address of 12-bytes memory block.

In case of writing to the characteristic only the first byte (command), it switches the context so one can read the value stored at location of interest.

For example, to store new name of the board, one need to write packet:
```
0x10 <5 bytes ascii>
```

To get firmware version, write 1 byte to the characteristic 0x20 and then read that characteristic.