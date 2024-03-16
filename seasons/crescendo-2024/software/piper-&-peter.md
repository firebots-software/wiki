# Piper & Peter: The Dynamic Duo

## Wait, what ARE Piper and Peter, exactly?

* Peter is the name for our intake-shooter mechanism.
* We have set the intake motors to run at 0.5 speed until our infrared sensor detects a note within 2 feet of it. Once this condition is true, the intake motors run at 0 speed and the preshooter motor runs for as many rotations as needed to move the note 3 more inches before stopping. Once driver input is given, the shooter motors run at a slower speed for a few seconds to warm them up before going to full speed. At the same time, the preshooter motor begins to run again to shoot the note.

## What kind of driver controls do we use here?

* When the driver presses on the left trigger, the note is loaded but not shot. This means that we intake the note, run the preshooter 3 inches, but we don't run the shooter motors yet. It's not until the right trigger is pressed by the driver that we finally shoot the note. This is to ensure that we're prepared for situations where we want to make sure we're aimed properly first before shooting.
* But what if we want to shoot immediately? If the left trigger hasn't already been pressed by the driver, the right trigger does all the things PLUS shooting when it's pressed. This is to ensure we're prepared for times when we want to shoot quickly.
