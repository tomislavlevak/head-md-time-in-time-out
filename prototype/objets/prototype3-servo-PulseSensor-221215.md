# Servo + PulseSensor

<img src="images-proto3\Proto3_Servo_PulseSensor.jpeg" >

Servo's angle incrementing by 1° each time Beatrate goes under 80BPM

<img src="images-proto3\Proto3_Servo_PulseSensor_demo.gif" width="60%" >

circuit

<img src="images-proto3\Proto3_circuit_PulseSensor_Servo.png" width="60%" >

code

```
#include <Servo.h>

#define USE_ARDUINO_INTERRUPTS true
#include <PulseSensorPlayground.h>

Servo myServo;

int a=0;
const int PulseWire = 0;
const int LED13 = 13;
int Threshold = 550;

PulseSensorPlayground pulseSensor;

void setup() {
    myServo.attach(9);

    Serial.begin(9600);  

    pulseSensor.analogInput(PulseWire);  
    pulseSensor.blinkOnPulse(LED13);     
    pulseSensor.setThreshold(Threshold);  

    if (pulseSensor.begin()) {
        Serial.println("We created a pulseSensor Object !");
        }
}

void loop() {
    int myBPM = pulseSensor.getBeatsPerMinute();

if (pulseSensor.sawStartOfBeat()) {
    Serial.println("♥  A HeartBeat Happened ! ");
    Serial.print("BPM: ")
    Serial.println(myBPM);
    
    if (myBPM<80 && myBPM>30){
        a = a +1;
        Serial.println(String("is moving: angle is: ") + a);
        }
}

delay(20);

myServo.write(a);
}