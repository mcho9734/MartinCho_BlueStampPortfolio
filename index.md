# 3-Joint Robotic Arm
My project is a 3-joint robotic arm that uses four servo motors to control four ranges of motion. The robotic arm is able to move side to side, up and down, forward and backward, and pick up objects. The servo motors are powered by a sensor shield and are controlled using the x and y axes on two joysticks of a controller.



| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
| Martin C | Troy High School | Mechatronics Engineering | Incoming Junior

**Replace the BlueStamp logo below with an image of yourself and your completed project. Follow the guide [here](https://tomcam.github.io/least-github-pages/adding-images-github-pages-site.html) if you need help.**

![Headstone Image](logo.svg)
  
# Final Milestone


<iframe width="560" height="315" src="https://www.youtube.com/embed/F7M7imOVGug" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

Final Milestone Key Details:
- What you've accomplished since your previous milestone
- What your biggest challenges and triumphs were at BSE
- A summary of key topics you learned about
- What you hope to learn in the future after everything you've learned at BSE

# Final Milestone Code
Here's where you'll put your code. The syntax below places it into a block of code. Follow the guide [here]([url](https://www.markdownguide.org/extended-syntax/)) to learn how to customize it to your project needs. 

```c++
void setup() {
  // put your setup code here, to run once:
  Serial.begin(9600);
  Serial.println("Hello World!");
}

void loop() {
  // put your main code here, to run repeatedly:

}
```

# Second Milestone


<iframe width="560" height="315" src="https://www.youtube.com/embed/C2Xco2PSx_w?si=evDUAN3PfTF7yNpN" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

Second Milestone Key Details:
- Technical details of what you've accomplished and how they contribute to the final goal
- What has been surprising about the project so far
- Previous challenges you faced that you overcame
- What needs to be completed before your final milestone 

# Second Milestone Code
Here's where you'll put your code. The syntax below places it into a block of code. Follow the guide [here]([url](https://www.markdownguide.org/extended-syntax/)) to learn how to customize it to your project needs. 

```c++
#include <Servo.h> // add the servo libraries
Servo myservo1; // create servo object to control a servo
Servo myservo2;
Servo myservo3;
Servo myservo4;
int pos1=90, pos2=90, pos3=90, pos4=90; // define the variable of 4 servo angle,and assign the initial value (that is the boot posture
//angle value)
const int right_X = A2; // define the right X pin to A2
const int right_Y = A5; // define the right Y pin to A5
const int right_key = 7; // define the right key pin to 7（that is the value of Z）
const int left_X = A3; // define the left X pin to A3
const int left_Y = A4; // define the left X pin to A4
const int left_key = 8; //define the left key pin to 8（that is the value of Z）
int x1,y1,z1; // define the variable, used to save the joystick value it read.
int x2,y2,z2;

void setup()
{
// boot posture
  myservo1.write(pos1);
  delay(1000);
  myservo2.write(pos2);
  myservo3.write(pos3);
  myservo4.write(pos4);
  delay(1500);
  pinMode(right_key, INPUT); // set the right/left key to INPUT
  pinMode(left_key, INPUT);
  Serial.begin(9600); // set the baud rate to 9600
}

void loop()
{
  myservo1.attach(3); // set the control pin of servo 1 to D3  dizuo-servo1-3
  myservo2.attach(5); // set the control pin of servo 2 to D5  arm-servo2-5
  myservo3.attach(6); //set the control pin of servo 3 to D6   lower arm-servo-6
  myservo4.attach(9); // set the control pin of servo 4 to D9  claw-servo-9
  x2 = analogRead(right_X); //read the right X value
  y2 = analogRead(right_Y); // read the right Y value
  z2 = digitalRead(right_key); //// read the right Z value
  x1 = analogRead(left_X); //read the left X value
  y1 = analogRead(left_Y); //read the left Y value
  z1 = digitalRead(left_key); // read the left Z value
  //delay(5); // lower the speed overall

  // claw
  claw();
  // rotate
  turn();
  // upper arm
  upper_arm();
  //lower arm
  lower_arm();
}

//claw//
void claw()
{
  //claw
  if(x1<50) // if push the left joystick to the right
  {
    pos4=pos4+3; 
    myservo4.write(pos4); //servo 4 operates the motion, the claw gradually opens. 
    delay(15);

    if(pos4>120) //limit the largest angle when open the claw 
    {
      pos4=120;
    }
  }

  if(x1>1000) ////if push the right joystick to the left 
  {
    pos4=pos4-3; 
    myservo4.write(pos4); // servo 4 operates the action, claw is gradually closed.
    delay(15);
    if(pos4<45) // 
    {
      pos4=45; //limit the largest angle when close the claw
    }
  }
}

// turn//
void turn()
{
  if(x2<50) //if push the right joystick to the let 
  {
    pos1=pos1+3; 
    myservo1.write(pos1); // arm turns left
    delay(18);
    if(pos1>180) //limit the angle when turn right 
    {
      pos1=180;
    }
  }

  if(x2>1000) // if push the right joystick to the right
  {
    pos1=pos1-3; 
    myservo1.write(pos1); //servo 1 operates the motion, the arm turns right. 
    delay(18);
    if(pos1<1) // limit the angle when turn left
    {
      pos1=1;
    }
  }
}

// lower arm//
void lower_arm()
{
  if(y2>1000) // if push the right joystick downward
  {
    pos2=pos2-2;
    myservo2.write(pos2); // lower arm will draw back
    delay(15);
    if(pos2<25) // limit the retracted angle
    {
      pos2=25;
    }
  }

  if(y2<50) // if push the right joystick upward
  {
    pos2=pos2+2;
    myservo2.write(pos2); // lower arm will stretch out
    delay(15);
    if(pos2>180) // limit the stretched angle
    {
      pos2=180;
    }
  }
}

//upper arm//
void upper_arm()
{
  if(y1<50) // if push the left joystick downward
  {
    pos3=pos3-2;
    myservo3.write(pos3); // upper arm will go down
    delay(15);
    if(pos3<1) //  limit the angle when go down 
    {
      pos3=1;
    }
  }
  if(y1>1000) // if push the left joystick upward
  {
    pos3=pos3+2;
    myservo3.write(pos3); // the upper arm will lift
    delay(15);
    if(pos3>135) //limit the lifting angle 
    {
      pos3=135;
    }
  }
}
```

# First Milestone


<iframe width="560" height="315" src="https://www.youtube.com/embed/HRW0X-JDqnY?si=iJHHUqSDNEeywt0l" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

- The robotic arm consists of an acrylic frame that is controlled using two joysticks that send input signals to the Arduino Uno board. The code on the Arduino IDE then takes this signal and converts it into a command for the servo motors to execute, such as moving the arm left 30 degrees. The servo motors are attached to the acrylic frame of the robotic arm and are powered by a sensor shield that is placed on top of the Arduino Uno's pins.
- For the first milestone, I plan on testing my servo motors and joysticks so I don't have problems later that cause me to dissasemble the whole arm.
- One minor challenge I faced was Arduino repeatedly giving me error messages when uploading code, but this was easily fixed by restarting my Arduino IDE.
- For my second milestone, I plan on completing the assembly and code for the robotic arm.

# First Milestone Code
Here's where you'll put your code. The syntax below places it into a block of code. Follow the guide [here]([url](https://www.markdownguide.org/extended-syntax/)) to learn how to customize it to your project needs. 

```c++
void setup() {
  // put your setup code here, to run once:
  Serial.begin(9600);
  Serial.println("Hello World!");
}

void loop() {
  // put your main code here, to run repeatedly:

}
```

# Schematics 
//delete text below later//
Here's where you'll put images of your schematics. [Tinkercad](https://www.tinkercad.com/blog/official-guide-to-tinkercad-circuits) and [Fritzing](https://fritzing.org/learning/) are both great resoruces to create professional schematic diagrams, though BSE recommends Tinkercad becuase it can be done easily and for free in the browser. 

![Servo Diagram](assets/images/FourServoDiagram.png)


# Bill of Materials

| **Part** | **Note** | **Price** | **Link** |
|:--:|:--:|:--:|:--:|
| LAFVIN Mechanical Arm Claw Kit | Contains the arduino board, sensor shield, servo motors, wires, joystick, and acrylic parts for the robotic arm | $53.99 | <a href="https://lafvintech.com/products/new-lafvin-4dof-acrylic-toys-robot-mechanical-arm-claw-kit-for-arduino-for-uno-r3-diy-robot-with-cd-tutorial"> Link </a> |
| Arducam | Provides video capabilities to the robotic arm | $Price | <a href="https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/"> Link </a> |
| HM10 Bluetooth Module | Provides bluetooth capabilities to the robotic arm, allowing it to be controlled using a phone | $10.99 | <https://www.amazon.com/DSD-TECH-Bluetooth-iBeacon-Arduino/dp/B06WGZB2N4?source=ps-sl-shoppingads-lpcontext&ref_=fplfs&psc=1&smid=AFLYC5O31PGVX> Link </a> |

# Other Resources/Examples
//maybe list websites used for this project. Ex. website used for arducam guide.//
- [Example 1](https://trashytuber.github.io/YimingJiaBlueStamp/)
- [Example 2](https://sviatil0.github.io/Sviatoslav_BSE/)
- [Example 3](https://arneshkumar.github.io/arneshbluestamp/)
