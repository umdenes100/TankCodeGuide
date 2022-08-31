

# Hotrod-Tanks
Navigational tanks created to supplement student coding curriculum in ENES100.

<p align="left">
  <img src="https://raw.githubusercontent.com/umdenes100/TankCodeGuide/master/img/IMG_0786.jpg" alt="Hotrod Tank" width="250">
  <p> The tank has swappable wheels to test out different wheels in prototyping. </p>
  <img src="https://raw.githubusercontent.com/umdenes100/TankCodeGuide/master/img/IMG_0788.jpg" alt="Internals and wiring" width="250">
  <p> A picture of the internal wiring of the tank</p>
  <img src="https://raw.githubusercontent.com/umdenes100/TankCodeGuide/master/img/IMG_0789.jpg" alt="Wifi module placement" width="450">
  <p> Place the 
wifi module on the tank, which is on the velcro.</p>
</p>

## Prerequesites
1. Have Arduino installed on your computer. The download link can be found [here](https://www.arduino.cc/en/software)
2. Download the ENES 100 library [here](http://enes100.umd.edu/libraries/enes100)

## Pin Connections
#### Romeo
A Romeo microcontroller is an Arduino that contains an integrated H bridge to deliver power to the motors. The motors are connected to the motor A and motor B sockets. The motors are controlled by 4 pins that are prewired as the following:
   
- M1 = 4
- E1 = 5
- E2 = 6
- M2 = 7

The motor (M) pins control the direction the motors spin while the enable (E) pins control the speed. You will need to set these pins accordingly
to make the tanks move.

  <img src="https://raw.githubusercontent.com/umdenes100/TankCodeGuide/master/img/romeo.png" alt="Sensor" width="500">

#### Ultrasonic Sensor
The ultrasonic sensor will detect objects in front of it, and output the distance. It has 4 pin connections:
- Vcc = 5V
- Trig = 8
- Echo = 9
- Gnd = Gnd

<img src="https://raw.githubusercontent.com/umdenes100/TankCodeGuide/master/img/ultrasonic.jpg" alt="Sensor" width="250">
  
  
#### Wifi Module
The wifi module connects to the vision system and delivers the last known position of the Aruco marker on your tank. It has 4 pin connections:
- Gnd = Gnd
- Vcc = 5V
- Tx = 10
- Rx = 11

<img src="https://raw.githubusercontent.com/umdenes100/TankCodeGuide/master/img/img_wifi.jpg" alt="Sensor" width="250">


## Programming the Tank
The side where the ultrasonic sensor sits is the front of the tank. You can upload your code by using the port located at the front of the tank.

### Motor Code
The code below will help you move your tank forward.
```
  //initializes or declares variables. Convention for hard coded values such as pins
  #define M1 4
  #define E1 5
  #define E2 6
  #define M2 7
  
  //define and int here do the same thing but since speed is a variable it is initialized using int
  int speed = 255; //Speed ranges from 0 to 255
  
  //only happens once
  void setup(){
  //loops through all 4 pins and dedicates them as outputs.
    for (int i = 4; i < 8; i++){
      pinMode(i, OUTPUT);
    }
  }
  
  //repeats forever unless end is specified
  void loop(){
    //calls function
    forward();
  }
  
  //creates a function to drive forward at full speed
  void forward(){
      //goes inside a function or the loop part
    analogWrite(E1, speed); // This puts the speed of motor 1 to 255
    digitalWrite(M1, HIGH); // This puts the spinning direction of the motor forward

    analogWrite(E2, speed); // Puts motor 2 speed to 255
    digitalWrite(M2, HIGH); // Puts motor 2 direction to forward
  }
```

### Ultrasonic Sensor Code
The code below will give you the distance detected in centimeters.
```
// ---------------------------------------------------------------- //
// Arduino Ultrasoninc Sensor HC-SR04
// Sourced from https://create.arduino.cc/projecthub/abdularbi17/ultrasonic-sensor-hc-sr04-with-arduino-tutorial-327ff6
// ---------------------------------------------------------------- //

#define echoPin 9 // attach pin D2 Arduino to pin Echo of HC-SR04
#define trigPin 8 //attach pin D3 Arduino to pin Trig of HC-SR04

// defines variables
long duration; // variable for the duration of sound wave travel
int distance; // variable for the distance measurement

void setup() {
  pinMode(trigPin, OUTPUT); // Sets the trigPin as an OUTPUT
  pinMode(echoPin, INPUT); // Sets the echoPin as an INPUT
  Serial.begin(9600); // // Serial Communication is starting with 9600 of baudrate speed
  Serial.println("Ultrasonic Sensor HC-SR04 Test"); // print some text in Serial Monitor
  Serial.println("with Arduino UNO R3");
}
void loop() {
  getDistance();
}

void getDistance(){
  // Clears the trigPin condition
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  // Sets the trigPin HIGH (ACTIVE) for 10 microseconds
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  // Reads the echoPin, returns the sound wave travel time in microseconds
  duration = pulseIn(echoPin, HIGH);
  // Calculating the distance
  distance = duration * 0.034 / 2; // Speed of sound wave divided by 2 (go and back)
  // Displays the distance on the Serial Monitor
  Serial.print("Distance: ");
  Serial.print(distance);
  Serial.println(" cm");
}
```
### Wifi Module code
The following code will get you to print the distance from the ultrasonic sensor onto the vision system.

```
// ---------------------------------------------------------------- //
// Arduino Ultrasoninc Sensor HC-SR04 and Wifi Module code
// Using Arduino IDE 1.8.7
// Using HC-SR04 Module
// Tested on 17 September 2019
// ---------------------------------------------------------------- //

#include "Enes100.h" //libraries are included at the very top of the program

#define echoPin 9 // attach pin D2 Arduino to pin Echo of HC-SR04
#define trigPin 8 //attach pin D3 Arduino to pin Trig of HC-SR04
#define txPin 10
#define rxPin 11

// defines variables
long duration; // variable for the duration of sound wave travel
int distance; // variable for the distance measurement
int arucoID = 32; //you will have to check the aruco marker's number on the vision system

void setup() {
  Enes100.begin("LTF>UTF", FIRE, arucoID, txPin, rxPin);//starts the connection to the vision system. The parameters are (Name, Mission, Aruco Marker, Tx pin, Rx pin)

  pinMode(trigPin, OUTPUT); // Sets the trigPin as an OUTPUT
  pinMode(echoPin, INPUT); // Sets the echoPin as an INPUT
  
  Enes100.println("Ultrasonic Sensor HC-SR04 Test"); // print some text in Serial Monitor
  Enes100.println("with Arduino UNO R3");
}
void loop() {
  getDistance();
}

void getDistance(){
  // Clears the trigPin condition
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  // Sets the trigPin HIGH (ACTIVE) for 10 microseconds
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  // Reads the echoPin, returns the sound wave travel time in microseconds
  duration = pulseIn(echoPin, HIGH);
  // Calculating the distance
  distance = duration * 0.034 / 2; // Speed of sound wave divided by 2 (go and back)
  // Notice that this now uses the wifi modules to print to the vision system.
  Enes100.print("Distance: ");
  Enes100.print(distance);
  Enes100.println(" cm");
}
```
## Coordinates and Movement Challenge: 
- Write an arduino program to get the coordinates of the aruco marker from the vision system. 
- Print it to the vision system.
- Use the ultrasonic sensor to detect obstacles and avoid them by turning.
- Use the location data from the vision system to find your way past the log or the limbo to the end.
- You can choose to do this however you like!
