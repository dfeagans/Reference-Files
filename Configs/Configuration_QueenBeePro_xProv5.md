# Configuration Details on 22/12/07

## References:
- [Source Video for Most of This Document](https://youtu.be/I2wec_L6j14)
- [General Tutorial on xPRO v5](https://makerhardware.net/knowledge-base/faqs/xpro-v5-faq/)

## My System Specifics:
- **NC Emergency Stop on NO Firmware:** This just means the button must be pushed to run and twisted out to stop it. Opposite of normal. Would be resolved with [different firmware](https://github.com/Spark-Concepts/xPro-V5/tree/main/Firmware) flash.
- **Slower Max Rates and Default Accelerations** - See below for why I did run particularly fast max rates or accelerations.
- **Steppers Failing to Energize** - I was warned by some people that the xPro v5 will sometimes fail to energize and hold the steppers on initial start-up when the usb cord is plugged in. They said their work around was not plugging in the usb cord until the system was powered up. I haven't observed this yet, but wanted to note it.

## Original Steps For Setting Up xProV5 and QueenBeenPro 1000mm x 1000mm:

- **Install Required Programs:**
    - Install [CP210x Drivers](https://www.silabs.com/developers/usb-to-uart-bridge-vcp-drivers?tab=downloads) first.
    - Install [CNCJS](https://github.com/cncjs/cncjs/releases), 64-bit Windows version.

- **Connecting via USB:** If you're plugged in via USB, look for the COM port that is labelled with "Manufacturer: Silicon Labs." It's likely the CNC xProV5 unit. Use the defualt 115200 baud rate.

- **Connecting via Wifi (Option):** It's possible to not use CNCjs and just connect to the webuUI that the xProV5 serves up on the hotspot it creates ([see here](https://github.com/Spark-Concepts/xPro-V5/wiki/Wifi_guide)). In short, the URL is http://192.168.0.1/, once you join it's "CNC_XPRO_V5" network.

- **Correcting Motor Directions:** I had to flip the Y and X motor directions to get them to agree with the following conventions: (Looking at Front of Machine Facing Rearward):  X+: Right / Y+: Away / Z+: Up.

        $stepper/dirinvert=XY

- **Keep Motors Energized** - This setting leaves the motors energized at all times the system is powered up. The main reason I did it was because my spindle and vacuum system was so heavy that it would slid down if they weren't energized at times. It also prevents moving when doing tool changes. ***I ACTUALLY SET THIS TO 1 FOR MOST OF MY TESTING SINCE IT WAS QUIETER***

        $stepper/idletime=255

- **MicroStepping and Steps/MM Settings** - I copied these settings from a video. They only did the X,Y,Z, but I did the A as well. I don't think that was necessary though since the firmware probably handles the A/Y2 thing.

        $X/microsteps=16
        $Y/microsteps=16
        $Z/microsteps=16
        $X/stepspermm=400
        $Y/stepspermm=400
        $Z/stepspermm=400
        $A/microsteps=16
        $A/stepspermm=400

- **Max Speed Setting and Acceleration** - Again, copied, and added the A axis myself as a precaution. 
    -   I had an issue here where my machine overload on the X and Y axis if I used the faster settings in the tutorial. Meaning I did NOT use these settings:

            $X/maxrate=4000
            $X/acceleration=300
            $Y/maxrate=4000
            $Y/acceleration=300
            $Z/maxrate=4000
            $Z/acceleration=300
            $A/maxrate=4000
            $A/acceleration=300

    - I did end up using these settings though, which are default for the acceleration and slower than default for the max rates:

            $X/maxrate=2500
            $X/acceleration=150
            $Y/maxrate=2500
            $Y/acceleration=150
            $Z/maxrate=2500
            $Z/acceleration=150
            $A/maxrate=2500
            $A/acceleration=150

- **See Current Axis Settings** - `$X/`, `$Y/`, `$Z/` will show the settings on each axis.
- **DTI Check** - I did a quick check with a DTI to make sure all the stepper settings (microsteps and steps per mm) were correct.
- **Enable Homing** - `$homing/enable=on`
- **Test Limit Switches** - Use `?` to query the systems status. It'll tell you if a limit switch is on. Confirm all of them work by holding each one and querying the status.
- **Test Emergency Stop Switch** Toggle your emergency stop and it should throw you a door open message. This is just to make sure it works before you start testing out the homing switches.
- **Check Homing Direction** - You obviously want the system moving to the homing switches when you hit the homing button. To check that, reset/unlock and then hit the homing button. Make sure each axis is moving towards them. My Z was fine but the X and Y had to be told to move towards the switches using `$homing/dirinvert=`, since they were previously set to `$homing/dirinvert=XY`. To check their setting before doing any changes, just do `$homing/dirinvert` first.
- **Set Maximum Dimensions** - After homing successfully, jogged the X and Y to figure out the max limits and then coded them in using the settings below. I wasn't particularly aggressive on maxing out the area though. Additionally, I did it based on where the actual cutting head was. Meaning I didn't push the spindle way over the front edge of the table since I won't need that all the time. The Z-axis I only went down 100mm because that puts the collet nut into the waste board mounting rails (based on the height I have my spindled mounted).

        $X/maxtravel=750
        %Y/maxtravel=750
        $A/maxtravel=750 (again, probably not necessary)
        $Z/maxtravel=100
- **Make Sure Limits Are On (Hard and Soft)** - Check the limit settings using `$limits/`. Mine showed that both hard and soft limits were on. They were actually both off by default but I turned them on using the commands below. The hard limit stops the machine if the limit switches are hit during operation. The soft limit triggers if a command requests to move outside the maxtravel limits. Reset/unlock afterwards to make sure it's taken effect.

        $limits/hard=on
        $limits/soft=on

- **Aligning Y and A/Y2 Axis** - I need to look into a better way to do this, but to make sure there was minimum binding by them being skewed to each other after homing the machine I used a digital calipers to measure each of those axis's stepper mounting plates to the carriage plate on the same side. Since I had the stepper idletime set to 1 I could adjust them by hand until they measured perfectly. Then I homed again, checked it, and they seemed good.
- **Stepper Motor Amperage** - The default run amps was set to 1.8 and default hold to 1.2. You can check that using `$X/current`, `Y/current`, etc.. My steppers were the 2.45 Nm NEMA 23's which are rated for 3 amps. His tutorial said to scale the rated amperage by 0.707. I assume that's something to do with a sin wave's peak voltage/current being 1/0.707 times the RMS voltage/current.

        I just left the currents as is for now, but they would be changed using following maximum settings for my 3amp motors:
            $X/current/run=2.12
            $Y/current/run/2.12
            $A/current/run=2.12
            $Z/current/run/2.12

- **VFD Set-Up** - Original set-up set the frequency using the front panel initially, instead of the RS485 modbus. Note that my spindle is an 110v, 2.2kw unit called "GDZ-80-2.2B. Originally, I had set the jumpers to connect the 2-3 (center and right) legs. That was because I was trying to use the front potentiometer originally, but didn't in the end. It was still set to 2-3 for all the below though. Otherwise, This was the settings process I used, remember to use *shift* to jump over units.
    
            PD013 - 8 (resets everything to factory defaults)
            PD001 - 0 (use the LCD display settings to control it)
            PD002 - 0 (use the LCD frequency to control motor)
            PD005 - 400 (max frequency, set first to enable setting other frequency settings)
            PD003 - 400
            PD004 - 400 
            PD007 - 0.5 (min frequency)
            PD008 - 110 (max voltage)
            PD009 - 15
            PD0190 -8 (min voltage)
            PD011 - 133.33 (min frequency)
            PD014 - 10 (accel time)
            PD015 - 10 (decel time)
            PD025 - 1 (starting method is frequncy tracking)
            PD141 - 110 (max volts)
            PD142 - 8 (sets max amps. One instruction said that the 2.2kw should have 10 here, but the bulkman 3d website for the 2.2kw, https://bulkman3d.com/product/spk-2200/, said that it was rated for 8 amps. I also saw lots of other comments online talking about having it set to 9 amps. Regardless, I used 8amps.
            PD143 - 2 (number of poles)
            PD144 - 3000 (revolutions at 50hz setting)
                     
    - After those settings are done: Un-plug, Connect Spindle, and then plug back in and set the *frequency* on screeen then Run. Note that since it's a 2 pole motor the RPM of the motor is the Hz setting * 60. Meaning that 133.33Hz min frequency equals 8000rpm min speed and 400Hz max frequency equals 24000rpm max speed.

## Wifi Set-Up:

- The xProV5 starts it Access Point/Hot Spot mode, so connect to the network it creates and use the default password (12345678). Then go to http://192.168.0.1/ and under the ESP3D tab change the Station SSID and Station Password to your networks. Then change the radio mode to Client Station so it knows to connect to your network. Then you can go to the static IP there to access the Web UI.

## Useful GRBL Commands:
The webUI has fewer buttons than the interface that's served up by the xProV5 itself when you first connect to it:
- **Reset** - instead of a clearly labelled button, you have to hit the refresh symbol button. Looking into the js code, it is sending the best reset command, equivalent to Ctrl-X. Specificaly, `SendRealtimeCmd(String.fromCharCode(0x18));`.
- **Unlock** - There's no unlock button in webui, so you have to send the gcode *$X* after reset.

## Facing Wasteboard:
- Bit used was Ø1" Surfacing Bit with 1/4" Shank from Amazon (BK-191028).
- Used https://www.intuwiz.com/plane.html to create the gcode for facing my wasteboard using the following settings:

            Center of Coordinates = 3 
            ZigZag X Selected
            Side a & b = 775           
            Tool Diameter = 25.4
            Y Overlap Percentage = 30
            Total Depth of Cut = 1
            Cut Depth Per Pass = 1
            Feed Rate (X,Y) = 2000
            Feed Rate (Z)   = 50
        
- Then I homed the machine, zeroed everything, navigated roughly to the center of the board, lowered the z until it just touched, zeroed only the z, homed, and then ran the resulting NC code.

## Fusion 306 Settings:
- **Post-Processor**: used the standard "Grbl / gbrl" post processor including with Fusion 360.
- **Safe Retract Setting** - set to 'Clearance Height' in the post-processor settings. Originally, the default had the Safe Retracts setting to "G28". That made the machine go back to the machine's reference position at the start and the end of the path. I believe the intention was to provide the ability to do tool changes then. I used "g28.1" to set that reference position at the same time I set my work position. To avoid having to remember that I switched to using the Safe Retracts setting as "Clearance Height" instead. That way I didn't risk forgetting. It's my understanding that the "G53" setting causes the machine to restract to the machine home position before and after the change. Again the intention is to provide repeatable tool change point. I have not tested that though.
- **Coolant** - Set to disabled under the Tool Path, since I don't have it. Prevents the unnecessary M7,M8(flood on),M9(flood off) commands from being added.
- **Spindle Speed** - Right now I don't have it hooked up to make use of it, but (as an example) the lines 'S14000 M3' turns the spindle on clockwise at 14,000 rpm. Later the line 'M5' stops the spindle.
- **End Code** - 'M30'.

  ## XYZ Probe:
  - I had the OpenBuilds XYZ Touch Probe Plus. I wired it using their striaghtforward instructions and made sure to have the jumper set to 5V inside the xPro V5 (it could be disconnected, all it does is make a light turn on when the probe shorts and some noise filtering.
  - [Wiring Reference](https://docs.openbuilds.com/doku.php?id=docs:xyzprobe:wiringgeneric)
  - ![image](https://github.com/dfeagans/Reference-Files/assets/4512270/9da505af-2826-4254-84e3-4d2d34470c41)
  - ![image](https://github.com/dfeagans/Reference-Files/assets/4512270/c31314f3-cdf7-42fe-9d74-2d3906792e35)
  - Testing: Send '?' to query the machine state and it if the probe isn't shorted it should just send back the normal '<Idle|MPos:0.000,0.000,0.000|FS:0,0|WCO:0.000,0.000,0.000>'. If it's shorted (the light will be green on the probe) you'll get back '<Idle|MPos:0.000,0.000,0.000|FS:0,0|Pn:P|Ov:100,100,100>'. Note the 'Pn:P' addition that indicates the probe is shorted. If you need to toggle it (say it showed 'Pn:P' when NOT shorted, then you check the setting for '$6' (it will either be 0 or 0) and switch it to the alternate setting using '$6=1' or '$6=0'.
  - Probe Block Dimensions: The offsets caused by the block had to be coded into the macro (below). I measured by block at 10mm X and Y offsets/thickness and 9mm Z offset.
  - Probe Direction: The code below is set-up to have the block on the bottom left corner of the project (it probes down, then to the rigt and the up to find the Z,X,Y respectively).
~~~
  (start with the end mill 15mm ABOVE the plate, about 15mm or less from bottom left corner.)

; Set user-defined variables
%ENDMILL_DIAMETER = 9.525	;in millimeters

%PROBE_BLOCK_Z = 9;Thickness of 3-axis probe in Z direction
%PROBE_BLOCK_Y = 10 ;Thickness of 3-axis probe in Y direction
%PROBE_BLOCK_X = 10 ;Thickness of 3-axis probe in X direction


G21	;make sure we're in mm
G91 ;Incremental mode
G38.2 Z-25 F75	;Probe Z
G0 Z2	;lift 2mm
G38.2 Z-25 F45	;Probe Z
G4 P0.1
G10 L20 P1 Z[PROBE_BLOCK_Z]	;Set Current Z as plate thickness
G4 P0.1
G0 Z3 ;lift Z 3mm
G0 X-25	;Move left 25mm
G0 Z-10	;Move down 10mm, should be 7mm below probe surface
G38.2 X30 F75	;Probe X to the right 30mm
G0 X-2
G38.2 X30 F45	;Probe X to the right 30mm
G4 P0.1
G10 L20 P1 X[-ENDMILL_DIAMETER/2 -PROBE_BLOCK_X]	;Set current X location as negative half the bit diameter
G4 P0.1
G0 X-20	;Move left 20mm
G0 Y-35	;Move forward 35mm
G90 G0 X5	;Move to X5 (absolute) - will put you 5mm to the right of left edge of stock
G91 ;incremental
G38.2 Y30 F75	;Probe Y
G0 Y-2
G38.2 Y30 F45	;Probe Y
G4 P0.1
G10 L20 P1 Y[-ENDMILL_DIAMETER/2 -PROBE_BLOCK_Y]	;Set current Y location as negative half the bit diameter - 7mm thickness
G4 P0.1
G0 Y-10	;Move Y-10
G0 Z10	;Move Z up 10mm, should be 8mm above probe plate
G90
G0 X0Y0	;Go to X0Y0
~~~
