# PhotonVision

## What is PhotonVision?

The goal of PhotonVision is to get the limelight to sense the AprilTags. PhotonVision is the only compatible software we found online that works with FRC. PhotonVision is essenstially OS that runs on the limelight. For more information visit their main website: [PhotonVision Main Site](https://docs.photonvision.org/en/latest/)


## Download PhotonVision

[Download Steps](https://www.balena.io/etcher/)

For the limelight, head on over to the [PhotonVision Gitub Page](https://github.com/photonvision/photonvision/releases). Get the latest release of PhotonVison. The name of the file you download should end with `image_limelight.xy`. 

## Flash PhotonVision to limelight

Use the [Balena Etcher](https://www.balena.io/etcher/) software to flash the image onto the limelight. Connect your computer to the micro-USB of the limelight. The computer cannot be a Windows 7 or Windows XP. MAKE SURE THE LIMELIGHT HAS NO POWER. Then open Balena Etcher in adminstrator. After doing so, click the Flash From File button. Select the PhotonVision image you downloaded. Then click select target. Select the limelight. Then flash. After flashing, set up [PhotonVision pipeline](photon-vision-pipeline.md).

## Documentation

Implement [these](https://docs.photonvision.org/en/latest/docs/programming/photonlib/getting-target-data.html) functions on your code.

## Constants

Since PhotoVision's mathematical distance calculations are unreliable, we had to use mathematical modeling using `tuner.py`. We realize that PhotonVision's distance calculations are linearly related to the actual distance. We are inputting pitch and yaw into two separate linear functions. We have 4 constants: Aagrim's constant, Yajwin's constant, Aryav's constant, and Ritvik's constant.
Here is a breakdown of what they do:

Linear equation for pitch: \\( y = m_1x+b_1 \\) (where x is pitch)

Linear equation for yaw: \\( y = m_2x+b_2 \\) (where x is yaw)

* Aagrim's constant: The \\( m_1 \\) value for the **pitch** linear function 

* Yajwin's constant: The \\( b_1 \\) value for the **pitch** linear function

* Aryav's constant: The \\( m_2 \\) value for the **yaw** linear function

* Ritvik's constant: the \\( b_2 \\) value for the **yaw** linear function


### Using `tuner.py`

It is a python script that does some trig with the inputs. Using numpy it returns a linear equation that relates PhotonVision data to real values. To use, collect data on five differnt known distances away from AprilTag. Collect the following data for each distance:

* PhotonVision reported distance from tag
* PhotonVision reported yaw
* PhotonVision reported pitch
* Horizontal distance from robot to tag (manually measured)
* Actual distance from tag (manually measured)

#### Inputing values

There are two variables called `pitchData` and `yawData`. For `pitchData` you need to line up the robot exactly infront of the AprilTag. And then you need to measure the distance between the AprilTag and the robot as well as what the robot thinks the pitch is. Then you input that into pitch data, as a list of points, the points are `[distance, reportedPitch],[distance, reportedPitch],[distance, reportedPitch]` as many tumes as you have data points. Then repeat a similar process with `yawData`, except the points follow the pattern of `[[straightDistToAprilTag,horizontalDistToAprilTag],reportedYaw]`. Just run the python file to get the values.

## What is pitch?

The pitch that photonVision returns is the difference in pitch between the triangle formed by center of sight of the camera to the wall and the pitch of triangle formed camera to AprilTag.
Here is an example:

![Pitch Example](photonVision_images/photonVisionPitchExample.png)

## PathPlanner 

We used PathPlanner to move the robot and align with the AprilTag by taking in the distance given from the the PhotonVision after doing all our mathematical stuff.  We ran into a major problem. The problem was that PathPlanner took the distance input only once and then used that same info to move everyother time. This resulted in the robot aligning good once, and then every other time it over or under shoots because its using the first data. What we did to fix this is that we moved PathPlanner code and the getting distance code into the `initialize()` method of a new Command called `MoveToTag`. We then passed the `MoveToTag` into a button on the PS controller. This means that evertime the button is clicked, a new `MoveToTag` instance is created and the `initialize()` method is called, creating a new PathPlanner with the new distance measurements. Then we called PathPlanners `execute()` in the `MoveToTag` `execute()`. This runs the PathPlanner.

