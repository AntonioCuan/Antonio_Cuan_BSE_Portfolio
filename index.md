# Obstacle Avoiding Robot
My project is an obstacle avoiding robot. It is a car-like robot equipped with an ultrasonic sensor.

| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
| Antonio C. | Homestead High | Computer Science and Electrical Engineering | Incoming Junior

![Headstone Image]
  
# Final Milestone
My final milestone is the increased reliability and accuracy of my robot. I ameliorated the sagging and fixed the reliability of the finger. As discussed in my second milestone, the arm sags because of weight. I put in a block of wood at the base to hold up the upper arm; this has reverberating positive effects throughout the arm. I also realized that the forearm was getting disconnected from the elbow servo’s horn because of the weight stress on the joint. Now, I make sure to constantly tighten the screws at that joint. 

[![Final Milestone]

# Second Milestone
My final milestone is the increased reliability and accuracy of my robot. I ameliorated the sagging and fixed the reliability of the finger. As discussed in my second milestone, the arm sags because of weight. I put in a block of wood at the base to hold up the upper arm; this has reverberating positive effects throughout the arm. I also realized that the forearm was getting disconnected from the elbow servo’s horn because of the weight stress on the joint. Now, I make sure to constantly tighten the screws at that joint.

[![Third Milestone]

# First Milestone
My first milestone was wiring the initila components for the robot and writing code to get them working. After getting familiar with the Arduino IDE and Arduino code, I wired the motor controller with two motors and the Arduino. Next, I ran demo code I found from a tutorial to test that the components worked. Then, I wrote code to run the motors so that they would make the robot move forwards, move backwards, turn left, and turn right.

[![First Milestone]

# Circuit Diagram
![motor circuit](https://user-images.githubusercontent.com/57155887/127692518-d34dd873-4641-4be8-a0a7-444d6f4f4f0d.png)

# Code
```Arduino
// motor 1 pins
int enA = 11;
int in1 = 13;
int in2 = 12;

// motor 2 pins
int enB = 3;
int in3 = 4;
int in4 = 2;

// ultrasonic sensor pins
int trigPin = 8;
int echoPin = 7;

// other variables
int speed = 128;
float speedOfSound = 0.034;

void setup()
{
  Serial.begin(9600);

  pinMode(enA, OUTPUT);
  pinMode(enB, OUTPUT);
  pinMode(in1, OUTPUT);
  pinMode(in2, OUTPUT);
  pinMode(in3, OUTPUT);
  pinMode(in4, OUTPUT);

  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
}

void setLeftMotorForward()
{
  digitalWrite(in1, HIGH);
  digitalWrite(in2, LOW);
}

void setLeftMotorBackward()
{
  digitalWrite(in1, LOW);
  digitalWrite(in2, HIGH);
}

void setRightMotorForward()
{
  digitalWrite(in3, LOW);
  digitalWrite(in4, HIGH);
}

void setRightMotorBackward()
{
  digitalWrite(in3, HIGH);
  digitalWrite(in4, LOW);
}

void setSpeed()
{
  analogWrite(enA, speed);
  analogWrite(enB, speed);
}

void moveForward()
{
  setLeftMotorForward();
  setRightMotorForward();
  setSpeed();
}

void moveBackward()
{
  setLeftMotorBackward();
  setRightMotorBackward();
  setSpeed();
}

void turnLeft()
{
  setLeftMotorBackward();
  setRightMotorForward();
  setSpeed();
}

void turnRight()
{
  setLeftMotorForward();
  setRightMotorBackward();
  setSpeed();
}

void stop()
{
  digitalWrite(in1, LOW);
  digitalWrite(in2, LOW);
  digitalWrite(in3, LOW);
  digitalWrite(in4, LOW);
}

float ultraSonicMeasure()
{
  // generate 10-microsecond pulse to TRIG pin
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  // measure duration of pulse from ECHO pin
  float duration_us = pulseIn(echoPin, HIGH);

  // calculate the distance
  float distance_cm = (speedOfSound * duration_us) / 2;

  return distance_cm;
}

void loop()
{

}
```
