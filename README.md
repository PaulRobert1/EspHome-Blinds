# EspHome-Blinds
open or close blinds/curtains etc using a esp8266, a limit switch and a servo motor

Your Blinds: Now Smarter Than Your Average Window Dressing!
Ever wished your blinds had a brain? Well, now they do! This ESPHome configuration turns your humble blinds into smart, obedient servants, all thanks to a microcontroller and a bit of clever coding.
What This Magical Configuration Does:
Remote Control: You can control your blinds from Home Assistant (or any app that talks to it) like a boss. Open, close, or stop them with a tap.
Physical Buttons: For those "I'm too lazy to grab my phone" moments, there are simple up/down buttons right on the device.
Smart Movement: Tap "Up" or "Down" once, and they'll automatically move to their extreme positions (fully open or fully closed). No holding required!
Auto-Correction (Thanks to Limit Switches!): Blinds can be divas, and sometimes they lose track of where they are. Limit switches act as their "reset button" for true position, making sure they always know when they've hit the top.
Calibration: You can tell your blinds where "fully closed" is and save it. They'll remember it, even if you unplug them!
The Star Players: What You'll Need
The Brains (Microcontroller):
Wemos D1 Mini (ESP8266): The little guy you're currently using. It's affordable, common, and has enough grunt for this job. Think of it as the caffeine-fueled intern of the smart home world.
Other ESP32 boards: If you decide to upgrade later, this config might need minor tweaks, but the core logic is transferable.
The Muscles (Servo Motor):
Standard Servo Motor (like SG90, MG996R, etc.): These are the workhorses that actually turn your blinds. You'll need one that's strong enough to move your blinds. Make sure it can handle the torque.
Important: The configuration uses esp8266_pwm which is designed for DC motors that might have an H-bridge or similar driver. If you are using a standard hobby servo (like SG90), you'll need to use the servo platform instead of output and servo.write instead of output.write. The current code is using the servo platform, so you're good there! Just ensure you have the right servo motor for your blinds.
The "I've Arrived!" Sensors (Limit Switches):
Microswitches: These are your best friends for absolute positioning. They're small, reliable, and have a satisfying "click." You'll need at least one, usually for the "home" position.
Types:
Lever Actuator: Good for when you can easily mount the switch so the blind's mechanism pushes a little lever.
Roller Lever: Similar, but the lever has a wheel, which can sometimes make contact smoother.
Button Style: If you can't mount a lever, a simple push-button style might work if you can arrange for something to press it.
Placement is KEY:
Top Limit Switch (as in your config, connected to D5): Mount this so that when the blinds are fully open, something on the blind mechanism presses this switch. This is your "zero" position.
Bottom Limit Switch (Optional, but recommended for ultimate perfection): You could add another one. Mount it so that when the blinds are fully closed, something on the blind mechanism presses this switch. This would allow you to implement a "stop at bottom" logic similar to what you have for the top.
Calibration Tutorial: "Teaching Your Blinds Who's Boss"
Let's get these blinds calibrated so they know their place in the world!
Step 1: The "Home" Positioning (The Grand Reset)
Wire it up! Make sure your limit switch for the top position is correctly wired to D5 and GND.
Upload the Config: Upload this latest YAML to your ESP8266.
Press "Up": Go to Home Assistant and tap the "Move Up" button.
Watch and Wait: Your blinds should start moving up.
THE MOMENT OF TRUTH: The blinds will continue going up. As soon as they physically hit the limit switch, they must stop. If they don't reach the switch, review the wiring and the inverted: true setting for the limit_switch_top.
Success! If they stop right at the limit switch, your system has correctly registered the current_pos as 0. You've successfully homed them!
Step 2: Setting the "Fully Closed" Position (The "Max Travel" Command)
Press "Down": Now, tap the "Move Down" button.
Observe the Counter: In Home Assistant, look at the "Blinds Position Counter" sensor. It will be going up from 0 (e.g., 1, 2, 3...).
Release at the Bottom: Keep an eye on your blinds. When they reach the exact position you consider "fully closed," release the "Move Down" button. Do NOT hold it until it jams. Release it just as it reaches the desired closed spot.
Save the Position: Immediately after releasing the button, go to Home Assistant and click the "Save This as Max Position" button.
Verification: The "Blinds Position Counter" should now show a larger number (e.g., 1250). This is your max_pos. The system now knows that 0 is fully open and 1250 is fully closed.
Step 3: Test the System Like a Pro!
Press "Up": The blinds should go all the way to the limit switch and stop. The counter should reset to 0.
Press "Down": The blinds should move down until the counter reaches your saved max_pos (e.g., 1250) and stop.
Press "Up" again: They should go back to the limit switch and stop.
Troubleshooting Tips:
Blinds jamming: If they jam instead of stopping, you might need to adjust max_pos to a slightly smaller number.
Still not reaching limit: Double-check wiring for the limit switch and ensure the inverted: true setting is correct for your switch type (Normally Open vs. Normally Closed).
Counter going wild: Make sure your interval is not too fast and the servo is receiving a stable 50Hz signal.
Enjoy your smart blinds – they're probably more reliable than your Uncle Gary at family gatherings!
