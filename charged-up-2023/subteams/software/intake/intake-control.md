# Programming the Intake 


## Intake Responsibilities: 
1. Code to allow driver to manually extend arm
* whenHeldRightBumper(extendArm(speed:1))
2. Code to allow driver to manually close piston
* whenClickedRightTrigger(extendPiston())
3. Code to allow driver to manually retract arm
* whenHeldLeftBumper(retractArm(speed:1)
4. Possibly code to automate process (depends on other subteams choices)
* whenClickedB(runIntakeProcess(cone))
* whenClickedA(runIntakeProcess(cube))
* Open piston to width of cube + more
* In 3 seconds (say) close piston around cube 

## Outtake Responsibilities:
1. Calculate arm extend distance and angle using trig for each different level of scoring
2. Create commands that correspond to low, mid, high score areas
* whenClickedLeftDpad(extendArm(low, speed:1)
* whenClickedRightDpad(extendArm(mid, speed:1)
* whenClickedUpDpad(extendArm(top, speed:1)
3. 2nd Driver Controller could have buttons for each level
4. Open pistons to release object
5. Automated in other commands
