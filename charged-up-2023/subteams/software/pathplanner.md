# PathPlanner

## **PathPlanner Github**
[Wiki](https://github.com/mjansen4857/pathplanner/wiki)

## **PathPlanner setup**
__Installing PathPlannerLib__

First, open VSCode, and in the project folder, type Cmd+Shift+P. Then select **WPILib: Manage Vendor Libraries** -> **Install new libraries (online)**. Next, paste the **Java/C++ Teams** link from [here](https://github.com/mjansen4857/pathplanner/wiki/PathPlannerLib:-Installing).

__PathPlanner App__

Although not necessary for TeleOp, for autonomous: install the PathPlanner app (available on Windows or Mac stores). Within the app, when prompted, select the `main` directory of your project folder. (__Note: the app will ask you to select the deploy directory, but from our experience, this causes directory issues that involve some refactoring. Regardless, we think this bug will be fixed soon))

In VSCode, select your `deploy` folder and ensure there is a `pathplanner` folder within it. If this folder is located somewhere else, refactor the intended directory within the app.

In the menu within the app, open the settings and select **Generate JSON** and **Holonomic Mode**, and ensure your robot dimensions are accurate (bumpers included).

## PathPlanner Editor
Although you can check the PathPlanner wiki for more info, here are some general tips:

Generally, use holonomic mode when creating paths. Although you can disable this at any time, we find working with holonomic mode is much easier for general control over the rotation of the robot.

When creating events (commands that happen at different points within your path, i.e. intaking an object), you will enter the event name in the stop event menu, and in your eventMap (found in `RobotContainer.java`), the name **must** be the same as what you entered within the app.

**IMPORTANT NOTE:** If any of the commands you run intermittently change the robot position in any way (e.g. `MoveToTag`, where vision tries to align the robot to the correct position for scoring), you must end your path with that command. If the auton requires you to move after finishing this command, build a trajectory in a DIFFERENT file that starts where you ended. Then, when adding these in `AutonPaths.java` (detailed below), make sure you add these files sequentially. A helpful naming convention is to label each portion of the entire auton with "part 1" , "part 2", etc.

Whenever you make changes to a path within the app, ensure the changes are actually reflected in the generated `.path` file. 

## Running a path
To run an **autonomous** path, follow this protocol.

Add the path(s) to the `AutonPaths.java` file. Add the following code, replacing `{ PathName }` with the name of your path(s) (without the `.path`):
```java
public static List<List<PathPlannerTrajectory>> { PathName } = makePPTList(
    "{ PathName }", "{ AnotherPathName}"
);
```

Next, in `RobotContainer.java`, locate the `initializeAutonChooser()` method and add the following code, replacing `{ PathName }` with the name of your path (without the `.path`): 
```java
autonChooser.addOption(
    "{ PathName }", 
    makeAuton(AutonPaths.{ PathName })
    );
```

Next, in the field variables, initialize the `eventMap` with the following code, replacing `{ Event 1 }` and `{ Event 2 }` with the **exact** names of the events found in the PathPlanner app:
```java
 public final Map<String, Command> eventMap = new HashMap<String, Command>() {{
    "{ Event 1 }", new ExampleCommand1(),
    "{ Event 2 }", new ExampleCommand2()
 }};
```









