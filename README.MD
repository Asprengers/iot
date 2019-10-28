# external server connection for nodemcu v3 esp8266
How to connect an external server connection to your nodemcu v3 esp8266 
In here it will be explained how to set up a nodemcu v3 esp8266 and send data from the server to an external device.  

## requirements
* nodemcu v3 esp8266
* arduino
* ultrasonic sensor hc-sr04
* mini servo sg90
* wires

## Background story
It rains quite often in the Netherland. During one of those rain showers, you are arriving home. Once you get home you must open the gate door. You take the key, which is very difficult, because everything is wet. In addition, the lock cannot withstand water. The next step is scanning your card at the cardreader. The pass is not recognised properly because it is not waterproof. This all takes a lot of time and in the meantime you are completely soaked.

There are various options for solving this problem. The solution for this has been chosen: simplifying life through an item that you always carry with you.

In this case it is a telephone, this can also be a smartwatch. There is a landmark in your phone, such as a nfc, that activates the cardreader's system. You can manage this in advance and set who can access it. The cardreader recognises that it is you at an appropriate distance and therefore it unlocks the door for you, opens it and this prevents the problem.

# Guide
## Step 1 Setting up the server
To start with the project we need to create a server. This can be done in the program Arduino. To create a server I will be using a template. A few adjustments have to be done with this.
* Change the wifi
* Enter your credentials 
* Create a specific route
* Test the code

More information 
https://www.youtube.com/watch?v=oWw2bUaQmLU


## Step 2 Setting up the database
The database I will be using is a mysql. I will be using this library to connect it:
https://github.com/ChuckBell/MySQL_Connector_Arduino. 
In this one I will replace the connection from setting up the server to the one I created in step 1. 

More information
https://planet.mysql.com/?tag_search=14086


## Step 3 Setting up the servo

For this project we need a servo motor. A servo motor has three wires: power, ground, and signal. The power wire should be connected to the 3V pin. The ground wire connected to a ground pin on the board. The signal pin should be connected to pin d5 on the board.

The code will be used from the template sweep that Arduino provided, with a slight few changes:


```
#include <Servo.h>

Servo myservo;  // create servo object to control a servo
// twelve servo objects can be created on most boards

int pos = 0;    // variable to store the servo position

void setup() {
  myservo.attach(D5);  // attaches the servo on pin d5 to the servo object
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
  myservo.attach(D4, OUTPUT);  // attaches the servo on pin 9 to the servo object
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

## Step 4 nfc

What I have so far is:
a server
Database with connection to a server
* motion from a servo
* a distance notice that triggers a servo if I am close

The only thing that I am missing right now is:
letting the nfc from your phone be the trigger to open the door if the distance is under 200 meters. 

If the database is connected, Arduino will pick up the external database and replaces the distance with the unique nfc. In the future I will implement this and make this prototype, working in real life.

A tutorial that I couldn't get to work has been found here, hopefully I can work on this in the future to find out why it hasn’t worked.
https://www.instructables.com/id/NFC-Card-Reader-Data-to-PhpMyAdmin/


## Summary 
There are various options for solving this problem. The solution for this has been chosen: simplifying life through an item that you always carry with you. For this example it is a nfc from your phone, that will open the door for you. 

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details

## Sources

https://howtomechatronics.com/tutorials/arduino/ultrasonic-sensor-hc-sr04/
https://www.arduino.cc/en/tutorial/sweep
