# Vision: "Literally Cooking"

## Goals for vision this year

This year, our goal was to use vision in conjunction with odometery readings to get an accurate Pose (or location of the robot on the field) even after being battered by other robots, which if odometry was left on its own, would have led to a more and more inaccurate Pose.

## Hardware Setup
Since we used an Orange Pi this year in contrast to Limelight 2, the hardware set up is much more complicated. 

## Installing PhotonVision and Photonlib

In contrast to last years Limelight 2, we chose the Orange Pi as our coprocessor of choice. Here is a general guide for the orange pi with photonvision: [Orange Pi Docs](https://docs.google.com/document/d/1DAPOnU2NfOp91UnMQnkhyiUAanlCDJ6zl_YsCkCMZdA/edit) Keep in mind, the docs are just notes that someone made while they were trying out the Orange Pi and all the info there should be taken with a grain of salt. We bought two Orange Pi 5's. On a microSD card, we flashed the image for the pi, which can be found here. [Latest Releases](https://github.com/PhotonVision/photonvision/releases). Make sure to download the img that is meant for the Orange Pi 5, not the 5 Plus. We used Balena Etcher to flash the image on the card. Put the SD card into the Pi. 
>**Please use caution to not pop out the sd card or unplug the Pi while the Pi is on. If you do manage to screw up and do this please refer to: [What to do if I popped out the SD card](#what-to-do-if-i-popped-out-the-sd-card)**
>

>**The orange pi is highly susceptible to overheating so please try add fans, heatsyncs and please don't add more than 2 cameras per orangepi**
>

After the pi boots up, the login is "pi" and the password is "raspberry". Next [Set up the static ID](#setting-up-static-ip-on-orangepi)

You can access the photon vision dashboard through your ip + port number. ```10.35.1.1:5800```

Next step is to install photonlib. Here is the general guide on how to install photonlib: [Photonlib Docs](https://docs.photonvision.org/en/latest/docs/programming/photonlib/adding-vendordep.html)
>**Please check if the photon vision version on your orangepi and the photonlib version are the same. Vision won't work if they don't match. [What to do if they dont match](#what-to-do-if-photonlib-version-doesnt-match-with-photon-vision-version)**
>

## What to do if I popped out the SD card

If you popped out your SD card or unplugged the Pi while it had power, then you did not follow my directions. You most likley lost all the calibrations and settings you had on the SD card because its now corrupted. You need to re-flash your sd card and recalibrate (if you didn't use calibdb).

## Setting up Static IP on OrangePI

On the OrangePI, do the following steps:

1. Type ```sudo vim /etc/netplan/01-static-ip.yaml```
2. Type
```yaml
network:
    version: 2
    renderer: networkd
    ethernets:
        eth0:
            dhcp4: no
            addresses:
                - 10.35.1.4/32
            routes:
                - to: default
                via: 10.35.1.1           
```

3. Run: `sudo netplan apply`
4. Restart the pi

## Calibration muy importante 
calibration is very important and you do two ways:
* through photon vision's UI [photonvision's UI](#photonvisions-ui)
* Calibdb (recommended)

## Calibrating on Photonvision's UI
it requires for you to print out the checkerboard given in the UI and then take hundreds of pictures in oblique angles, far and close until you fill up a lot of the screen in the dots ~500 photos

* if you take too many photos, it doesn't load upper limit of photos is around 700
* Make sure you dont move the board too fast otherwise the calibration will fail

## Calibrating on Cadlibdb
You need to print out their pattern. Plug in the camera to your computer and go to [put link for Calibdb here](). Match up your print out to the outline they display on the screen. Hold still while you image lines up with the image on the screen. It will automatically take the photo. Keep taking the photos and it will stop automatically. Then export the json, and in the photonvision Cameras tab, you can import the file. There will be a button.

## Setup Web UI of PhotonVision
Change the resolution of the camera to the resolution of the calibration. Also click auto exposure to be be on. Dont forget to upload calibration. In processing mode, click 3d. Then go to the AprilTag tab, change the tag family to the family used in your years game. Head over to Output and click 'Do Single-Target Estimation' and make sure 'always do single-target estimation is off'


## Straight line distance code
```java
public Transform3d getTransformToTarget() {
    if (!pipeline.hasTargets()) {
      return new Transform3d(0.0, 0.0, 0.0, new Rotation3d());
    }
    return bestTarget.getBestCameraToTarget();
  }

  public double get3dDist() {
    return getTransformToTarget().getTranslation().getNorm();
  }
  ```

## Pose3d (single target-- not really needed when multitag exists but is useful )
```java
public Pose3d getRobotPose3d() {
    if (!pipeline.hasTargets()) {
      return savedResult;
    }
    Optional<Pose3d> tagPose = aprilTagFieldLayout.getTagPose(bestTarget.getFiducialId());
    if (tagPose.isEmpty()) {
      return savedResult;
    } else {
      savedResult =
          PhotonUtils.estimateFieldToRobotAprilTag(
              getTransformToTarget(), tagPose.get(), camToRobot);
    }
    return savedResult;
  }
```

## Multitag 
```java
public Optional<EstimatedRobotPose> getMultiTagPose3d(Pose2d previousRobotPose) {
    photonPoseEstimator.setReferencePose(previousRobotPose);
    return photonPoseEstimator.update();
  }
```

## Kalman filter
Kalman filter combines with odomotery with pose of the robot and you can supply matrices (specifies the degree trust in the vision) to get a better picture of where the robot is relative to the field. 
In Robot.java:

In field:
```java
private static Matrix<N3, N1> visionMatrix = new Matrix<N3, N1>(Nat.N3(), Nat.N1());
```
In robot init:
```java
visionMatrix.set(0, 0, 0);
    visionMatrix.set(1, 0, 0.2d);
    visionMatrix.set(2, 0, Double.MAX_VALUE);
```
In periodic:

```java
m_robotContainer.doTelemetry();
    Optional<EstimatedRobotPose> frontRobotPose =
        frontVision.getMultiTagPose3d(driveTrain.getState().Pose);
    if (frontRobotPose.isPresent()) {
      driveTrain.addVisionMeasurement(
          frontRobotPose.get().estimatedPose.toPose2d(), Timer.getFPGATimestamp(), visionMatrix);
    }
    Optional<EstimatedRobotPose> sideRobotPose =
        sideVision.getMultiTagPose3d(driveTrain.getState().Pose);
    if (sideRobotPose.isPresent()) {
      driveTrain.addVisionMeasurement(
          sideRobotPose.get().estimatedPose.toPose2d(), Timer.getFPGATimestamp(), visionMatrix);
    }
```

## Camera Mount
It is **IMPERATIVE** that you look at camera position when designing the robot, so talk to design and work with them to figure where the camera would be most useful where it can see what it needs to see most, if not all the time. This also gives you the time to have the potential to add multiple cameras.