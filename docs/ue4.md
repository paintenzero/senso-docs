# Unreal Engine 4 Plugin

## Getting started guide

You can download Senso Plugin for UE4 from [here](https://senso.me/dev/downloads/senso-plugin-ue4.zip). Once downloaded you should put it into Plugins directory inside your Project folder.  
Restart UE4 editor with your project and go to `Settings -> Plugins`. Check if **Senso Gloves Plugin** under **Input Devices** has been enabled.  

Now you need to create three new blueprints:
* GameMode
* Blueprint that inherits ASensoPlayerController. Call it for example *BP_SensoPlayerController*
* Blueprint that inherits ASensoGlovesPawn. Call it for example *BP_GlovesPawn*

![Create blueprint example](ue4img/create_blueprint.png)

Now open Blueprint editor for just created GameMode. Set Default Pawn Class to *BP_GlovesPawn* and Player Controller Class to *BP_SensoPlayerController*.

![Set GameMode](ue4img/set_gamemode.png)

Now open `Settings -> Project Settings -> Maps & Modes` and set Default GameMode to newly created GameMode blueprint.  
Next you need to open Blueprint editor for *BP_GlovesPawn* and add *Event Senso Pose Changed*.

![Event Senso Data Received](ue4img/event_senso_data_received.png)

From now on you can use Senso Data and pose your pawn's hand. For details and example usage of Senso Data

## Sample Project

Sample project can be found [here](https://senso.me/dev/downloads/senso-sample-proj-ue4.zip). It consists of two hands from Epic's VR Template that are controlled using Senso Driver.

## Explanation of BP_GlovesPawn in sample project

First of all we are saving initial hands rotations to add it to rotations we receive from Senso Driver. It is done in Event BeginPlay. Then we implement Event Senso Pose Changed. As this event is triggered on any pose changed, we need to branch based on pose type.

![ue4img/bp_event_senso_pose_changed.png]

For each branch we need to setup pointers to meshes, arrays and variables to use later.

![ue4img/branch_variables.png]

After pointers were set we set world rotation of the hand and wrist using pose data we received. Please note we only needed to combine rotators of hand with initial rotations in the example. If you happen to need combine rotators for a wrist you can do similar way as for hand mesh.

![u4img/bp_set_palm_and_wrist.png]

Setting fingers' pose is a little bit harder. For each finger there is a pitch and yaw rotation angles (roll is not used as usually you can't rotate your finger around that axis). That angles are stored in two arrays whose indices starts from 0 for thumb to 4 for little finger. There is a helper function for setting finger's pose using those two angles. The function is called "Set Finger Bones".
The main idea is to apply pitch angle to first finger's bone only. At the same time the yaw angle is divided by (bones count - 1) and is applied proportionally to each finger's bone. However if the yaw angle is negative, we need to only set first finger's bone rotation.

![ue4img/set_fingers_bone.png]

That is how we can pose a hand using data received from Senso Driver.

## Class references

References for Senso Plugin classes can be found [here](https://senso.me/dev/ue4/reference)
