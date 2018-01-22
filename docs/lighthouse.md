## Getting started with Lighthouse and Senso

To start using lighthouse tracking you will need to connect to Senso gloves first. This procedure is the same as running Senso Glove without external tracking (see [Home page]()). After you connected a glove and successfully calibrated it you will want to enable Lighthouse (LH) support in the bottom of the app.

![SensoUI lighthouse tracking enable](/img/driver/LH_enable.png)

## Using SteamVR's room setup

SteamVR's room setup files are used automatically by Senso UI when you enable Lighthouse tracking. The location of files are read from file `%LOCALAPPDATA%\openvr\openvrpaths.vrpath`. To get proper coordinates please make sure it is parsed properly. "SteamVR room setup is used" _(TBD, screenshot required)_ will be written in Senso UI lighthouse section.

![SensoUI SteamVR room setup success](/img/driver/roomsetup_parsed.png)

## Calibration gesture

We do not use OpenVR API but use our own recalculations instead. To minimize offset between our tracking space and SteamVR we have a special gesture. Just make V sign with your hand and bring it to your head (HMD) with palm towards face. Once position is calculated and stabilized the offset between HMD and Glove will be calculated and applied to the coordinates.

![Senso SteamVR calibration gesture](/img/driver/LH_calibration_gesture.gif)

## Starting script

To make it easier to use Senso Gloves with lighthouse we have made convinient `run_with_lh.cmd` file to start both BLE server and SensoUI with enabled lighthouse support.

## Sample project

Sample project can be found at [github](https://github.com/paintenzero/SensoLH) or can be downloaded as ZIP from [Senso website](https://senso.me/downloads/Unity/sample-lighthouse-proj.zip). Please take a look at README of the repository.

## Known issues

If you have *poor tracking quality* you can try to place your lighthouse base stations along one wall (and not in diagonal as it is proposed by Steam). This way gloves would "see" two basestations most of the time you point at that wall direction and the tracking should be better. It is really hard to triangulate glove position using only one basestation so any occlusion of one of lighthouse basestations breaks the tracking. We are working hard to make it work with one basestation.