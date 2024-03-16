# Intake Control


## Working with Design for Initial Design
The original design for the arm includes a main claw controlled by pistons opening and closing. Then, there are attached motors to circular grippers so that it rotates when the cone is grabbed so that it is rotated to an upright position.

Intake Responsibilities: 
1. Code to allow driver to manually extend arm.
whenHeldRightBumper(extendArm(speed:1))
2. Code to allow driver to manually close piston.
whenClickedRightTrigger(extendPiston())
3. Code to allow driver to manually retract arm.
whenHeldLeftBumper(retractArm(speed:1)
4. Possibly code to automate process (depends on other subteams choices)
whenClickedB(runIntakeProcess(cone))
whenClickedA(runIntakeProcess(cube))
5. Open piston to width of cube and more.
6. In 3 seconds (say) close piston around cube.

Ideas we have:
* Pre established angles measured using gyroscope -> like if you press a button, no matter where you are, if you want to go to 90 degrees you go to that position

* Arm can extend and retract

* Claw opens at the sameish width always for the cube (9 ½ inches ± ¼ in) -> because it has a curve, it needs to be at least 9 ½ inches minimum to have enough grip