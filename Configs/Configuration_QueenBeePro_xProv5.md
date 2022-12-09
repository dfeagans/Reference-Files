# Configuration Details on 22/12/07

## Referneces:
- [Source Video for Most of This Document](https://youtu.be/I2wec_L6j14)
- [General Tutorial on xPRO v5](https://makerhardware.net/knowledge-base/faqs/xpro-v5-faq/)
## My System Specifics:
- **NC Emergency Stop on NO Firmware:** This just means the button must be pushed to run and twisted out to stop it. Opposite of normal. Would be resolved with [different firmware](https://github.com/Spark-Concepts/xPro-V5/tree/main/Firmware) flash.
- **Slower Max Rates and Default Accelerations** - See below for why I did run particularly fast max rates or accelerations.

## Original Steps For Setting Up xProV5 and QueenBeenPro 1000mm x 1000mm:

- **Install Required Programs:**
    - Install [CP210x Drivers](https://www.silabs.com/developers/usb-to-uart-bridge-vcp-drivers?tab=downloads) first.
    - Install [CNCJS](https://github.com/cncjs/cncjs/releases), 64-bit Windows version.

- **Connecting via USB:** If you're plugged in via USB, look for the COM port that is labelled with "Manufacturer: Silicon Labs." It's likely the CNC xProV5 unit. Use the defualt 115200 baud rate.

- **Connecting via Wifi (Option):** It's possible to not use CNCjs and just connect to the webuUI that the xProV5 serves up on the hotspot it creates ([see here](https://github.com/Spark-Concepts/xPro-V5/wiki/Wifi_guide)). In short, the URL is http://192.168.0.1/, once you join it's "CNC_XPRO_V5" network.

- **Correcting Motor Directions:** I had to flip the Y and X motor directions to get them to agree with the following conventions: (Looking at Front of Machine Facing Rearward):  X+: Right / Y+: Away / Z+: Up.

        $stepper/dirinvert=XY

- **Keep Motors Energized** - This setting leaves the motors energized at all times the system is powered up. Prevents moving when doing tool changes. ***I ACTUALLY SET THIS TO 1 FOR MOST OF MY TESTING SINCE IT WAS QUIETER***

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

            $X/maxrate=1000
            $X/acceleration=50
            $Y/maxrate=1000
            $Y/acceleration=50
            $Z/maxrate=1000
            $Z/acceleration=50
            $A/maxrate=1000
            $A/acceleration=50
   
- **DTI Check** - I did a quick check with a DTI to make sure all the stepper settings (microsteps and steps per mm) were correct.
- **Enable Homing** - `$homing/enable=on`
- **Test Limit Switches** - Use `?` to query the systems status. It'll tell you if a limit switch is on. Confirm all of them work by holding each one and querying the status.
- **Test Emergency Stop Switch** Toggle your emergency stop and it should throw you a door open message. This is just to make sure it works before you start testing out the homing switches.
- **Check Homing Direction** - You obviously want the system moving to the homing switches when you hit the homing button. To check that, reset/unlock and then hit the homing button. Make sure each axis is moving towards them. My Z was fine but the X and Y had to be told to move towards the switches using `$homing/dirinvert=`, since they were previously set to `$homing/dirinvert=XY`. To check their setting before doing any changes, just do `$homing/dirinvert` first.
- **Set Maximum Dimensions** - After homing successfully, jogged the X and Y to figure out the max limits and then coded them in using the settings below. I wasn't particularly aggressive on maxing out the area though. Additionally, I did it based on where the actual cutting head was. Meaning I didn't push the spindle way over the front edge of the table since I won't need that all the time. The Z-axis I only went down 100mm because that puts the collet nut into the waste board mounting rails (based on the height I have my spindled mounted).

        $X/maxtravel=750
        %Y/maxtravel=750
        $A/maxtravle=750 (again, probably not necessary)
        $Z/maxtravel=100

- **Aligning Y and A/Y2 Axis** - I need to look into a better way to do this, but to make sure there was minimum binding by them being skewed to each other after homing the machine I used a digital calipers to measure each of those axis's stepper mounting plates to the carriage plate on the same side. Since I had the stepper idletime set to 1 I could adjust them by hand until they measured perfectly. Then I homed again, checked it, and they seemed good.
- **Stepper Motor Amperage** - The default run amps was set to 1.8 and default hold to 1.2. You can check that using `$X/current`, `Y/current`, etc.. My steppers were the 2.45 Nm NEMA 23's which are rated for 3 amps. His tutorial said to scale the rated amperage by 0.707. I assume that's something to do with a sin waves peak voltage/current being 1/0.707 the RMS voltage/current.
