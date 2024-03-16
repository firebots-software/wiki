# Swerve Drive

The swerve drive is a type of drive base that allows a robot to move in all directions by rotating the angle of each wheel independently.

## Architecture

Right now, we have two subsystems related to the Swerve Drive, one associated with each module and one for the entire system. We also have a Swerve Joystick Command which binds the joystick to moving for teleop.

### Swerve Module

`SwerveModule.java` contains code pertaining to each "module", i.e two motors– the translational motor and rotational motor– and its functionality includes setting the wheel to a speed and angle.

Each module also has a turning encoder; in our case, we use a CANCoder, and it's absolute. This allows us to set a position and allow the wheel to rotate using a PID controller.

Nothing other than the Swerve Subsystem should use the Swerve Module class itself; instead, it should interface with the Swerve Subsystem.

### Swerve Subsystem

`SwerveSubsystem.java` combines four modules into one subsystem. It acts as the interface for all movement; you can set the module states by calling `setModuleStates(SwerveModuleState[4])`, and find pose and heading with the provided getters.

> The swerve subsystem's coordinate plane is +x as forward and +y as left. The Swerve Joystick Command takes in a front-back function and left-right function to reduce confusion, as joystick inputs are usually +x corresponding to right and +y being down.

### Swerve Joystick Command

`SwerveJoystickCmd.java` does seven things:

1. receives joystick input
1. normalizes input
1. sets input to 0 if in deadband range
1. smooths the input
1. constructs ChassisSpeeds from the joystick input
1. converts it into individual module states before 
1. sends the module states to the Swerve Subsystem

> The Swerve Joystick Command takes a front-back function and a left-right function; front and left are positive speeds, while back and right are negative speeds; for most controllers, you need to do `-controller.getRawAxis(axis)`.

It needs to be initialized in RobotContainer to be able to accept input. An example is provided below.

```java
swerveSubsystem.setDefaultCommand(new SwerveJoystickCmd(
    swerveSubsystem,
    () -> -controller.getRawAxis(1),
    () -> -controller.getRawAxis(0),
    () -> controller.getRawAxis(2),
    () -> !controller.getRawButton(3))
);
```
