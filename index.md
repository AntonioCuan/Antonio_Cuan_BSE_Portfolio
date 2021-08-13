# Obstacle Avoiding Robot
My project is an obstacle avoiding robot. It is a car-like robot equipped with an ultrasonic sensor.

| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
| Antonio C. | Homestead High | Computer Science and Electrical Engineering | Incoming Junior

![Headstone Image](https://user-images.githubusercontent.com/57155887/129388210-3451fbc2-02ac-45d5-bdfa-0635ccae48ef.JPG)

# Presentation
<iframe width="560" height="315" src="https://www.youtube.com/embed/7R9eO95h3bM" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

# Final Milestone
 


# Second Milestone
My second milestone was adding the ultrasonic sensor to the robot and programming the robot to avoid obstacles using the sensor's capabilities. I first programmed the ultrasonic sensor to sense how far away objects were and tested that it worked. I also put tape around each of the robot's wheels to lessen the friction between the the wheels and the ground. The robot had trouble turning without the tape, but turning was smooth with the tape. Then, I added the ultrasonic sensor to the robot and wrote code for the robot to avoid obstacles. I tested this with the robot, and it worked! Finally, I wrote code for the robot to alternate the direction it turned for each obstacle it encountered. This was more complicated that I initially thought, but after a lot of thinking and testing, I was able to get it to work.

<iframe width="560" height="315" src="https://www.youtube.com/embed/60f36BvJsg8" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

# First Milestone
My first milestone was wiring the initial components for the robot and writing code to get them working. First, I wired the motor controller with two motors and the Arduino. Next, I ran demo code I found from a tutorial to test that the components worked. Then, I wrote code to run the motors so that they would make the robot move forwards, move backwards, turn left, and turn right.

<iframe width="560" height="315" src="https://www.youtube.com/embed/7vULrbBvDaE" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

# Circuit Diagram
![motor circuit](https://user-images.githubusercontent.com/57155887/128399452-40fbbfae-4158-4255-9c41-0c4fe3b1fa13.png)

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

// other constants
int speed = 204;
float speedOfSound = 0.034;
float turnDistance = 30; // in cm

// other variables
float obstacleDistance;

// variables for alternating turns
bool toggleReferenceTime = true;
unsigned long referenceTime, elapsedTime;
int minForwardTime = 500;
bool directionRight = true;
bool hasTurned = false;

void setup()
{
  pinMode(enA, OUTPUT);
  pinMode(enB, OUTPUT);
  pinMode(in1, OUTPUT);
  pinMode(in2, OUTPUT);
  pinMode(in3, OUTPUT);
  pinMode(in4, OUTPUT);

  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
}

void setRightMotorForward()
{
  digitalWrite(in1, LOW);
  digitalWrite(in2, HIGH);
}

void setRightMotorBackward()
{
  digitalWrite(in1, HIGH);
  digitalWrite(in2, LOW);
}

void setLeftMotorForward()
{
  digitalWrite(in3, HIGH);
  digitalWrite(in4, LOW);
}

void setLeftMotorBackward()
{
  digitalWrite(in3, LOW);
  digitalWrite(in4, HIGH);
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

void turnRight()
{
  setLeftMotorForward();
  setRightMotorBackward();
  setSpeed();
}

void turnLeft()
{
  setLeftMotorBackward();
  setRightMotorForward();
  setSpeed();
}

void stop()
{
  digitalWrite(in1, LOW);
  digitalWrite(in2, LOW);
  digitalWrite(in3, LOW);
  digitalWrite(in4, LOW);
}

void testDirections()
{
  moveForward();
  delay(5000);
  stop();
  delay(2000);
  moveBackward();
  delay(5000);
  stop();
  delay(2000);
  turnRight();
  delay(5000);
  stop();
  delay(2000);
  turnLeft();
  delay(5000);
  stop();
  delay(2000);
}

float ultraSonicMeasure()
{
  // generate 10-microsecond pulse to TRIG pin
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  // measure duration of pulse from ECHO pin
  float duration = pulseIn(echoPin, HIGH);

  // calculate the distance in cm
  float distance = (speedOfSound * duration) / 2;

  return distance;
}

void moveAvoidObstacles()
{
  obstacleDistance = ultraSonicMeasure();
  if (obstacleDistance <= turnDistance)
  {
    turnRight();
  }
  else
  {
    moveForward();
  }
}

void complexMoveAvoidObstacles()
{
  obstacleDistance = ultraSonicMeasure();
  if (obstacleDistance <= turnDistance)
  {
    if (directionRight)
    {
      turnRight();
    }
    else if (!directionRight)
    {
      turnLeft();
    }

    referenceTime = millis();

    hasTurned = true;
  }
  else
  {
    moveForward();

    if (hasTurned)
    {
      if (toggleReferenceTime)
      {
        referenceTime = millis();
        toggleReferenceTime = false;
      }

      elapsedTime = millis() - referenceTime;
      if (elapsedTime >= minForwardTime)
      {
        directionRight = !directionRight;
        toggleReferenceTime = true;
        hasTurned = false;
      }
    }
  }
}

void loop()
{
  complexMoveAvoidObstacles();
}
```
