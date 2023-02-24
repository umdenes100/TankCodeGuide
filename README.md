

# Tank V3
Navigational tanks created to supplement student coding curriculum in ENES100.

<p align="left">
  <img src="https://raw.githubusercontent.com/umdenes100/TankCodeGuide/master/img/IMG_0814.jpg" alt="Tank" width="350">
  <p> The tank has swappable propulsion modules, including a few seen here. </p>
  <img src="https://raw.githubusercontent.com/umdenes100/TankCodeGuide/master/img/IMG_0817.jpg" alt="Internals and wiring" width="250">
  <p> In order to remove the propulsion modules, remove the wheels by turning counter clockwise and then slide the module out.</p>

## Prerequesites
1. Have Arduino installed on your computer. The download link can be found [here](https://www.arduino.cc/en/software)
2. Download the ENES 100 library [here](http://enes100.umd.edu/libraries/enes100)
3. Download the Tank library [here](http://enes100.umd.edu/libraries/tank)

## Programming the Tank
The front of the tank is marked by the forward facing arrow.

### Motor Code
The code below will help you move your tank forward.
```
#include "Tank.h"
  void setup(){
      Tank.begin();
  }
  
  //repeats forever unless end is specified
  void loop(){
    //turns on both motors to full speed (-255 to 255 are acceptable PWM values)
    Tank.setRightMotorPWM(255);
    Tank.setLeftMotorPWM(255);
    delay(1000);
    Tank.turnOffMotors();
  }
```

### Ultrasonic Sensor Code
```
#include "Tank.h"
void setup() {
  Tank.begin();
}
void loop() {
  Tank.setRightMotorPWM(pwm);
}
```
### Wifi Module code
The following code will allow you to connect to the vision system, and prints a message.

```
#include "Enes100.h" //libraries are included at the very top of the program
#include "Tank.h"

void setup() {
  //This function connects the tank to the vision system. The TX and RX pins of the tanks are 52 and 50 respectively.
  Enes100.begin("Alec B Lahr", MATERIAL, arucoID, 52, 50);

 delay(200);
  
  Enes100.println("Sucessfully connected to the Vision System"); // print some text in Serial Monitor
}

```
## Coordinates and Movement Challenge: 
- Write an arduino program to get the coordinates of the aruco marker from the vision system. 
- Print it to the vision system.
- Use the ultrasonic sensor to detect obstacles and avoid them by turning.
- Use the location data from the vision system to find your way past the log or the limbo to the end.
- You can choose to do this however you like!
