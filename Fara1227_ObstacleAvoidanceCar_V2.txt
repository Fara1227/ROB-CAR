
#include <Servo.h>  //servo library
Servo myservo;      // create servo object to control servo

int Echo = A4;
int Trig = A5;
int redLed1 = 11;
int redLed2 = 12;

#define ENA 5
#define ENB 6
#define IN1 7
#define IN2 8
#define IN3 9
#define IN4 10
#define carSpeed 250
int rightDistance = 0, leftDistance = 0, middleDistance = 0;

bool BT_EN = false;

void forward() {
  analogWrite(ENA, carSpeed);
  analogWrite(ENB, carSpeed);
  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, HIGH);
  digitalWrite(redLed1, LOW);
  digitalWrite(redLed2, LOW);
  Serial.println("Forward");
}

void back() {
  analogWrite(ENA, carSpeed);
  analogWrite(ENB, carSpeed);
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, HIGH);
  digitalWrite(IN3, HIGH);
  digitalWrite(IN4, LOW);
  digitalWrite(redLed1, LOW);
  digitalWrite(redLed2, LOW);
  Serial.println("Back");
}

void left() {
  analogWrite(ENA, carSpeed);
  analogWrite(ENB, carSpeed);
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, HIGH);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, HIGH);
  digitalWrite(redLed1, LOW);
  digitalWrite(redLed2, LOW);
  Serial.println("Left");
}

void right() {
  analogWrite(ENA, carSpeed);
  analogWrite(ENB, carSpeed);
  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, HIGH);
  digitalWrite(IN4, LOW);
  digitalWrite(redLed1, LOW);
  digitalWrite(redLed2, LOW);
  Serial.println("Right");
}

void stop() {
  digitalWrite(ENA, LOW);
  digitalWrite(ENB, LOW);
  digitalWrite(redLed1, HIGH);
  digitalWrite(redLed2, HIGH);
  Serial.println("Stop!");
}

//Ultrasonic distance measurement Sub function
int Distance_test() {
  digitalWrite(Trig, LOW);
  delayMicroseconds(2);
  digitalWrite(Trig, HIGH);
  delayMicroseconds(20);
  digitalWrite(Trig, LOW);
  float Fdistance = pulseIn(Echo, HIGH);
  Fdistance = Fdistance / 58;
  return (int)Fdistance;
}

void setup() {
  myservo.attach(3, 700, 2400); // attach servo on pin 3 to servo object
  Serial.begin(9600);
  pinMode(Echo, INPUT);
  pinMode(Trig, OUTPUT);
  pinMode(redLed1, OUTPUT);
  pinMode(redLed2, OUTPUT);
  pinMode(IN1, OUTPUT);
  pinMode(IN2, OUTPUT);
  pinMode(IN3, OUTPUT);
  pinMode(IN4, OUTPUT);
  pinMode(ENA, OUTPUT);
  pinMode(ENB, OUTPUT);
  stop();
}

void loop() {
  if (Serial.available() > 0) {
    char newChar = Serial.read();
    Serial.print("New Char: ");
    Serial.println(newChar);
    if (newChar == 'f') {
      BT_EN = false;
    }
    else if (newChar == 'e') {
      BT_EN = true;
    }
    
    if (BT_EN == true) {
      if (newChar == 'u') {
        forward();
      }
      else if (newChar == 'd') {
        back();
      }
      else if (newChar == 'l') {
        left();
      }
      else if (newChar == 'r') {
        right();
      }
      delay(100);
      stop();
    }
  }

  if (BT_EN == false) {
    auto_ObstacleAvoidance();
  }
}

void auto_ObstacleAvoidance() {
  myservo.write(90);  //setservo position according to scaled value
  delay(500);
  middleDistance = Distance_test();

  if (middleDistance <= 40) {
    stop();
    delay(500);
    myservo.write(10);
    delay(1000);
    rightDistance = Distance_test();

    delay(500);
    myservo.write(90);
    delay(1000);
    myservo.write(180);
    delay(1000);
    leftDistance = Distance_test();

    delay(500);
    myservo.write(90);
    delay(1000);
    if (rightDistance > leftDistance) {
      right();
      delay(360);
    }
    else if (rightDistance < leftDistance) {
      left();
      delay(360);
    }
    else if ((rightDistance <= 40) || (leftDistance <= 40)) {
      back();
      delay(180);
    }
    else {
      forward();
    }
  }
  else {
    forward();
  }
}

void BT_Control() {
  
}
