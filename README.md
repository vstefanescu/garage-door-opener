Garage Door Opener
This project enables remote control of your garage door using an Arduino board, a breadboard, wires, and a simple remote control setup. It's powered by a portable power bank for convenience and is enclosed within a makeshift cardboard housing.

Features
Remote Control: Control your garage door remotely using a simple remote control device.
LED Indication: An LED indicator provides visual feedback, lighting up when the door is opening and turning off when the door is closing.
Portability: The project is powered by a power bank, making it easy to use without the need for a direct power source.
Cardboard Enclosure: The components are housed in a DIY cardboard enclosure for protection and aesthetics.

Code Overview
Here is a brief overview of the Arduino code used in this project:

#include <IRremote.h>
#include <Stepper.h>

// Define constants and pins
const int stepsPerRevolution = 1000;
Stepper myStepper(stepsPerRevolution, 8, 10, 9, 11);

const int receiverPin = 2;
const int delayy = 8;
IRrecv irrecv(receiverPin);
decode_results results;

int aux = 0;
int speed = 50;

const int redPin = 6;
const int greenPin = 5;
const int bluePin = 7;

void setup() {
  // Initialize stepper motor and IR receiver
  myStepper.setSpeed(speed);
  irrecv.enableIRIn();
  Serial.begin(9600);

  // Initialize LED pins
  pinMode(redPin, OUTPUT);
  pinMode(greenPin, OUTPUT);
  pinMode(bluePin, OUTPUT);
}

// LED control functions
void ledOn(){
    digitalWrite(greenPin, HIGH);   // Turn on the green LED
}

void ledOff(){
    digitalWrite(greenPin, LOW);   // Turn off the green LED
}

void loop() {
  if (irrecv.decode(&results)) {
    if (results.value == 0xFFFFFFFF && aux == 0) {
      for (int i = 0; i < stepsPerRevolution / 2; i++) {
        myStepper.step(1);
        delay(delayy);
        aux = 1;
        ledOn();
      }
    } else if (results.value == 0xFFFFFFFF && aux == 1) {
      for (int i = 0; i < stepsPerRevolution / 2; i++) {
        myStepper.step(-1);
        delay(delayy);
        aux = 0;
        ledOff();
      }
    }
    irrecv.resume();
  }
}
