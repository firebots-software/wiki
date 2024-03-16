# Kalman Filter for Vision & Encoders/Gyro Odometry Integration

Odometry is perhaps the most important aspect of software programming in the FIRST Robotics Competition. In FRC, odometry refers to the usage sensors on a robot to create an estimate of its position on the field. Specifically, in the 2024 "Crescendo" game, odometry is necessary for the aiming of shots into the speaker from any location. It is also important for consistency in the autonomous period (i.e. the intake of notes and repeatable auton paths).

However, with the default odometry (which uses ***ONLY*** encoder measurements and the gyro), the odometer (pose estimator) is susceptible to extreme drift (how far the predicted or estimated measurement is away from the true state of the system)---which occurs due to wheel slip---caused by terrain imbalances and collisions with other entities on the field.

To compensate for this, external measurements (measurements that are not directly dependent on the reference measurement). In this case, an external measurement of the robot's pose is the vision pose estimator. Because it is completely independent from the encoder and gyro measurements, the vision pose estimator could correct incorrect default odometry estimations should a collision happen. To fuse these measurements optimally, the Kalman Filter can be used. 

A Kalman Filter is a state observer that combines information about a system's behavior, as well as external measurements, to create an optimal estimate of the system's true state. However, before it can be applied, the following assumption must be made: the system measurements as well as the external measurements vary like that of a Gaussian distribution. Though a complicated linear algebra & statistics / system identification explanation does exist, it is not optimal nor necessary to include in this wiki. Simply explained, given the independent variance measurement of both the system (reference) measurer and external measurers, the Gaussian distributions can be merged into a third Gaussian distribution that is the optimal measurement and has a lesser variance than both the original measurements.

This filter can be further tuned with heurestics. For example, the variance (accuracy in this case) of the vision pose estimator is dependent on the distance away from the camera to the April Tag that is used for the estimation. The farther the camera is away from the April Tags, the harder it is for PhotonVision to determine a precise bounding box -> which translates to the pose estimation. Thus, the robot's odometer should **TRUST** the vision measurement less the farther the tag (that is being used) is. The parent function used by us to describe this (distance from tag : inverse trust in vision) relationship is: $\text{trust} = a \cdot b^{\text{distance}}$ (exponential). This was selected due to its "spiking" behavior, which accurately describes the dip in trust at farther distances away. 

Ultimately, because we trusted x measurements from the photon vision pose estimator more, we changed the base of the exponential function to fit this specific feature:

***ROBOT.JAVA***

```
      double xKalman = 0.01 * Math.pow(1.15, distToAprilTag);

      double yKalman = 0.01 * Math.pow(1.4, distToAprilTag);

      visionMatrix.set(0, 0, xKalman);
      visionMatrix.set(1, 0, yKalman);
```

This is the code that was implemented to set the variance of the vision pose estimator in standard deviations (units are meters). In other words, the higher the value was in the matrix, the less trust there was in its measurement.