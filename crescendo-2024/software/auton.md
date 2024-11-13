# Autonomous: How We Achieved a Successful 3 Note Auton

## PathPlanner

- Easy way to see and make paths for auton.
- Can also be used during teleop so that the driver has an easier job.
- In its own app which is connected to the VScode.

## How it Works

PathPlanner is seperated into 2 sections: autos and paths. The paths are like singular lego blocks that you use to create a cool build. On the other hand, autos are like the final build you make with these lego blocks. The paths help you seperate the overarching path you want to make into seperate sections which can be used later on when making new autos. On top of that, you can include Commands such as Intake or Shoot within PathPlanner which will run the method that is made within the actual code. An example of that is located in the integration section of this page. 

Here is an example of how to set up the drive system that utilizes the PathPlanner (assumes that the code already has these methods)

The methods needed: 

* **getPose** - Returns the current robot pose as a Pose2d.
* **resetPose** - Resets the robot's odometry to the given pose.
* **getRobotRelativeSpeeds** or **getCurrentSpeeds** - Returns the current robot-relative ChassisSpeeds. This can be calculated using one of WPILib's drive kinematics classes.
* **driveRobotRelative** or **drive** - Outputs commands to the robot's drive motors given robot-relative **ChassisSpeeds**. This can be converted to module states or wheel speeds using WPILib's drive kinematics classes.

## How to configure for Swerve

```java
public class DriveSubsystem extends SubsystemBase {
  public DriveSubsystem() {
    // All other subsystem initialization
    // ...

    // Load the RobotConfig from the GUI settings. You should probably store this in your Constants file
    RobotConfig config;
    try{
      config = RobotConfig.fromGUISettings();
    } catch (Exception e) {
      // Handle exception as needed
      e.printStackTrace();
    }

    // Configure AutoBuilder last
    AutoBuilder.configure(
            this::getPose, // Robot pose supplier
            this::resetPose, // Method to reset odometry (will be called if your auto has a starting pose)
            this::getRobotRelativeSpeeds, // ChassisSpeeds supplier. MUST BE ROBOT RELATIVE
            (speeds, feedforwards) -> driveRobotRelative(speeds), // Method that will drive the robot given ROBOT RELATIVE ChassisSpeeds. Also optionally outputs individual module feedforwards
            new PPHolonomicDriveController( // PPHolonomicController is the built in path following controller for holonomic drive trains
                    new PIDConstants(5.0, 0.0, 0.0), // Translation PID constants
                    new PIDConstants(5.0, 0.0, 0.0) // Rotation PID constants
            ),
            config, // The robot configuration
            () -> {
              // Boolean supplier that controls when the path will be mirrored for the red alliance
              // This will flip the path being followed to the red side of the field.
              // THE ORIGIN WILL REMAIN ON THE BLUE SIDE

              var alliance = DriverStation.getAlliance();
              if (alliance.isPresent()) {
                return alliance.get() == DriverStation.Alliance.Red;
              }
              return false;
            },
            this // Reference to this subsystem to set requirements
    );
  }
}
```

## Integration with Robot Code

### AutoBuilder

- AutoBuilder is configured inside the Drivetrain Subsystem.

    - This includes setting PID.

- Autons are set inside RobotContainer within the `getAutonomousCommand()` method.
- Choosing an auton can be done using AutoBuilder inside the method, refer to `2024-crescendo` code to see how.
- To run the auton command that was loaded, use `.schedule()` in Robot.java

### Adding Name Commands

This is also done within RobotContainer's constructor:

```java
NamedCommands.registerCommand("Trigger Shot", new ArmToAngleCmd(angle, m_arm));
```

- PathPlanner automatically saves the autos and paths as json files within the robot code.
