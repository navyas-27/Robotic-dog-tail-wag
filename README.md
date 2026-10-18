# Robotic-dog-tail-wag
A simulated robot dog built on an ESP32 in the Wokwi online simulator. A servo motor acts as the dog's tail, and an HC-SR04 ultrasonic sensor acts as its nose. The dog reacts to how close you are by changing how it wags its tail.

Wokwi project:https://wokwi.com/projects/478123078237746177

How it behaves
Distance from the "nose"	Dog's reaction
Over 50 cm	Slow, lazy wag
10 to 50 cm	Fast, happy wag
Under 10 cm	Tail stops and the red LED turns on (the dog is startled)
Button pressed (bonus)	Dog "sits": tail freezes in the middle for 3 seconds

The measured distance (in cm) is printed to the Serial Monitor about five times per second.

Components
ESP32 development board
Micro servo (the tail)
HC-SR04 ultrasonic distance sensor (the nose)
Red LED with a 220 ohm resistor (the startled light)
Push button (the "sit" command)
Wiring
Part	ESP32 pin
HC-SR04 TRIG	GPIO 5
HC-SR04 ECHO	GPIO 18
Servo signal (PWM)	GPIO 13
LED (through 220 ohm resistor)	GPIO 26
Push button (other leg to GND)	GPIO 27

Power pins (VCC/V+) and GND connect to the matching rails on the ESP32.

How it works
Measuring distance: The ESP32 sends a 10 microsecond pulse on TRIG. The sensor emits an ultrasonic burst and holds ECHO high until the echo returns. The ESP32 times that pulse with pulseIn() and converts it to centimetres using time * 0.0343 / 2 (speed of sound, divided by two for the round trip).

Non-blocking timing: Instead of delay(), the code uses millis() timers. This lets the sensor reading, the tail movement and the button check all run in the same loop without freezing each other, so the tail reacts instantly when the distance changes.

Wagging: The tail moves in small 2 degree steps between 45 and 135 degrees. The wait between steps (30 ms for lazy, 5 ms for happy) sets the wag speed.

Button: The button uses INPUT_PULLUP, so pressing it reads LOW. A press centres the tail and freezes the loop logic for 3 seconds.
Files
sketch.ino: the program
diagram.json: the circuit wiring for Wokwi
libraries.txt: the ESP32Servo library
Run it

Open the Wokwi link, press the green Play button, and drag the distance slider on the ultrasonic sensor to watch the dog react.
