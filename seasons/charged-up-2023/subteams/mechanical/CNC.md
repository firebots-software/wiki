# CNC: Stepcraft 840 CK
* ### [Official Stepcraft CNC Manual](https://www.stepcraft-systems.com/images/SC-Service/Anleitungen-EN/EN-Operating-Instructions-SC2-v5-1.pdf)
* ### [HF500 Spindle Manual](https://www.stepcraft-systems.com/images/SC-Service/Anleitungen-EN/EN-Operating-Instructions-HF350-500-v7.pdf)
____
## G-Code:

G-Code is the language the CNC uses to execute programs. Here is a cheat sheet of the most commmonly used syntax!
| Syntax      | Description |
| :-----------: | :-----------: |
|G90|Start CNC program with abs coord. (always)|
| G0xyz          | Move to (x,y,z) with max feedspeed       |
| G1xyz|Move to (x,y,z) at set feedspeed        |
| M3|Spindle on (clockwise), CNC specific        |
| M5| Spindle Off        |
|S|Set Spindle speed (S1000)|
|F|Set feedrate (coresponding to G1)|

Example Program:
<br>```G90```----ABS Positioning
<br>```M3 S20000```---- Started spindle and set speed to 20000
<br>```G1 F800``` ---- Here I set my custom feedrate to 800
<br>```G1 X200Y400Z-5``` ---- Moved spindle to a coordinate at my set feedrate
<br>```G0Z0``` ---- Moved spindle up back to Z=0 at max feedrate
<br>```M5``` ---- Stopped Spindle
<br>(coordinates are in millimeters)
___
## Fusion 360 and Step Files 
Fusion 360 is a CAM (computer aided manufacturing) software, where we give it a drawing of the piece we wish to cut, tell it what we want to cut, and our CNC information, so it can give us a file with hundreds of lines of G-code.
<br> 
<br>It is not often that we write G-Code manually for a cut, no matter how simple it is. Exceptions include facing non-metal parts (MDF, polycarbonate, wood).
<br>
<br> [Here](https://youtu.be/wxFUNtJwiW0) is a detailed video on how to use Fusion 360 with a similar CNC to what we have. Also, it explains how to use the UCCNC software, which is where we input the G-Code file from Fusion (ignore the design part of the video).
 <br>
 <br>Our process for build season with the CNC is pretty straightforward. On the parts list, there will be certain parts marked with "CNC". After identifying the part, then design should provide you with a step file for the part, which can be imported into Fusion 360. After the cut, then mark the item as "Complete" in the parts tracker.
 ______
 ## Settings/Speeds for Fusion
 <br>
 <br> **The following speeds are for spindles with one flute -- for more flutes check the manual or the CNC book**
 
 
 <br> 
 
 ###  Plywood Settings

 | Diameter (mm)    | Spindle Speed (rpm) | Feed Rate (mm/s)|
| :-----------: |:-----------: | :------:|
|1|9500|9.5 | 
| 2| 4800|4.8 |
| 3|3200| 3.2|
| 4|2400|2.4 |
| 5| 1900| 1.9|
|6|1600| 1.6|

### MDF Settings 
 | Diameter (mm)    | Spindle Speed (rpm) | Feed Rate (mm/s)|
| :-----------: |:-----------: | :------:|
|1|15900|15.9 |
1.5|10600 | 10.6
2 | 8000 | 8
3| 5300| 5.3
4| 4000| 4
5 | 3200| 3.2
6 | 2700| 2.7

### Aluminum Settings
 | Diameter (mm)    | Spindle Speed (rpm) | Feed Rate (mm/s)|
| :-----------: |:-----------: | :------:|
1| 63700| 63.7
2| 31800| 31.8
3| 21200 | 21.2
4| 15900| 15.9
5| 12700| 12.7

*Please note that our spindle only goes up to 20,000 rpm, which means that for our 3.5 mm endmill should be set to around 20,000 rpm.*