# Swerve Drive General Information & Tips

## Parts of the Swerve Drivebase:
    - the actual base
    - 4 individual swerve modules

## Parts of each Swerve Module:
    - Driving Motor
    - Turning Motor
    - Built-in driving encoder (relative)
    - Built-in turning encoder (relative)
    - CANcoder (purchased) (absolute)

## Overview:
    The Swerve Drivebase is controlled through two subsystems. Because each swerve module is composed of the same components, a lot of the code is reusable. Therefore, it makes sense to have a seperate subsystem that represents the attributes and functions of each individual module. The other subsystem is the overall subsystem that initializes the 4 modules and applies methods in the perspective of the entire drive base.

## The Swerve Module Class:
    The parameters of the constructor are the different port numbers 