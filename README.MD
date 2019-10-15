# Manual
This project revolves about simplifying your life .

## Background story
It rains quite often in the Netherland. During one of those rain showers, you are arriving home. Once you get home you must open the gate door. You take the key, which is very difficult, because everything is wet. In addition, the lock cannot withstand water. The next step  is scanning your card at the cardreader. The pass is not recognised properly because it is not waterproof. This all takes a lot of time and in the meantime you are completely soaked.

## Solution
There are various options for solving this problem. The solution for this has been chosen: simplifying life through an item that you always carry with you.

In this case it is a telephone, this can also be a smartwatch. There is a landmark in your phone, such as a unique serialnumber, that activates the cardreader's system. You can manage this in advance and set who can access it. The cardreader recognises that it is you at an appropriate distance and therefore it unlocks the door for you, opens it and thus prevents the problem of being in the rain for a long time.

If you have guests on the floor, you can activate an alternative setting. The alarm goes off by means of a motion sensor and you can control it via an app on your phone. You receive a call and you can see who it is. The door is opened at a distance. This can also happen if you are not at home yourself.

For this project the prototype is built in Arduino. In this document you will find a guide how to access it. The service works between cardreader, motion sensor, servo and an app.


# Guide
## Step 1 Setting up the servo

For this project we need a servo motor. A servo motor has three wires: power, ground, and signal. The power wire should be connected to the 5V pin. The ground wire connected to a ground pin on the board. The signal pin should be connected to pin 9 on the board.

I will be using it like this example:

The code will be used from the template sweep that Arduino provided, with a slight few changes:


```
#include <Servo.h>

Servo myservo;  // create servo object to control a servo
// twelve servo objects can be created on most boards

int pos = 0;    // variable to store the servo position

void setup() {
  myservo.attach(9);  // attaches the servo on pin 9 to the servo object
}

void loop() {
  for (pos = 0; pos <= 90; pos += 1) { // goes from 0 degrees to 90 degrees
    // in steps of 1 degree
    myservo.write(pos);              // tell servo to go to position in variable 'pos'
    delay(15);                       // waits 15ms for the servo to reach the position
  }
  for (pos = 90; pos >= 0; pos -= 1) { // goes from 90 degrees to 0 degrees
    myservo.write(pos);              // tell servo to go to position in variable 'pos'
    delay(15);                       // waits 15ms for the servo to reach the position
  }
}

```

By adding the 90 degrees, it will have the movement that a door makes.

## Step 2 Adding distance sensor 

To have a sensor that locates the distance I will be using an ultrasonic sensor hc-sr04. This sensor registers all the objects in the distance and will result in an action. The action must be opening the door. 

By adding the code to the document it will look like this:

```
#include <Servo.h>

Servo myservo;  // create servo object to control a servo
// twelve servo objects can be created on most boards

int pos = 0;    // variable to store the servo position

void setup() {  
  myservo.attach(9, OUTPUT);  // attaches the servo on pin 9 to the servo object
  pinMode(echoPin, INPUT);
}

void loop() {
  digitalWrite(trigPin, LOW);  // Added this line
  delayMicroseconds(2); // Added this line
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10); // Added this line
  digitalWrite(trigPin, LOW);
  duration = pulseIn(echoPin, HIGH);
  distance = (duration/2) / 29.1;
  if (distance <200) {  // This is where the LED On/Off happens
    for (pos = 0; pos <= 90; pos += 1) { // goes from 0 degrees to 180 degrees
    // in steps of 1 degree
    myservo.write(pos);              // tell servo to go to position in variable 'pos'
    delay(15);                       // waits 15ms for the servo to reach the position
  }
}
  else { 
    for (pos = 90; pos >= 0; pos -= 1) { // goes from 180 degrees to 0 degrees
    myservo.write(pos);              // tell servo to go to position in variable 'pos'
    delay(15);                       // waits 15ms for the servo to reach the position
  }
    Serial.print(distance);
    Serial.println(" cm");
  }
  delay(500);
 
long duration, distance;
  
}

```
What’s new? 
If you have reached your distance under 200 meter it will trigger the servo

## Step 4 Future goals

What I have so far is:
* motion from a servo
* a distance notice that triggers a servo if I am close

The only thing that I am missing right now is:
* letting the unique serial number from your phone be the trigger to open the door if the distance is under 200 meters. 
* This could be solved with the external app where people sign themselves up for. If the database is connected, Arduino will pick up the external database and replaces the distance with the unique serial number.

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details

## Sources

https://howtomechatronics.com/tutorials/arduino/ultrasonic-sensor-hc-sr04/
https://www.arduino.cc/en/tutorial/sweep
