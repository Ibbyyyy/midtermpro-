---
title: Project
layout: home
---

# DIY Tachometer at home



This is a school project I have completed with a partner, this project is a homemade tachometer and you could use a tachometer to measure rpms of any type of rotating object.


More specifically, we intended to make this tachometer to measure the rpms of an exercise bike and measure the rpms.

# List of tools and items needed for this project

- Screwdriver
- Arduino UNO
- L2C backpack( a small display)
- IR sensor ( to read the rpms)
- Small piece of tape
- A exercise bike( or anything that has a rotating component to it) 
- Jumpers( small wires)

  You could edit and tweak things to make it more complicated and sophisticated if you'd like

# Process
First step is to acquire the tools that I have listed above.
Second step is to connect the arduino UNO with the L2C backpack into the right inputs, go by the following (GND -> GND),(vcc -> 5v),(SCL -> A5), the connections I have just listed are between the arduino and the lcd display 16x1.
Third step is to connect the IR sensor to the arduino aswell, (GND -> GND), (OUT -> 2),(vcc -> 3.3V)

![Sizzling Sango-Wolt](https://github.com/user-attachments/assets/716332c1-2f31-4ffb-8200-fec852f2ebba)

The following image is a visual representation of the connections between the arduino UNO and the l2c display.

Fourth step is to place the IR sensor facing towards the rotating object

Next you have to place a piece of tape that is a bright color in which the IR sensor can tell when it comes in its field of view.

Then you have to connect the arduino UNO with a PC or any device that can launch the arduino software in which you can write up your code in.

This is the following code I have used

#include <LiquidCrystal.h>

// Initialize LCD (pins: RS=12, E=11, D4=5, D5=4, D6=3, D7=2)
LiquidCrystal lcd(12, 11, 5, 4, 3, 2);

// IR Sensor pin
const int irPin = 7;

// Variables for RPM calculation
unsigned long lastTime = 0;
unsigned long pulseTime = 0;
int rpm = 0;
volatile unsigned long pulseCount = 0;
const unsigned long sampleTime = 2000; // Sample time in milliseconds

// Variables for display alternation
bool showTitle = true;
unsigned long displayToggleTime = 0;
const unsigned long toggleInterval = 2000; // Toggle display every 2 seconds

void setup() {
  // Set up LCD for 16x1 display
  lcd.begin(16, 1);
  lcd.clear();
  
  // Set up IR sensor pin
  pinMode(irPin, INPUT);
  
  // Attach interrupt
  attachInterrupt(digitalPinToInterrupt(irPin), countPulse, FALLING);
}

void loop() {
  // Calculate RPM every sampleTime milliseconds
  if (millis() - lastTime >= sampleTime) {
    // Detach interrupt while calculating
    detachInterrupt(digitalPinToInterrupt(irPin));
    
    // Calculate RPM
    rpm = (pulseCount * 60 * (1000/sampleTime));
    
    // Reset pulse count
    pulseCount = 0;
    
    // Record time and reattach interrupt
    lastTime = millis();
    attachInterrupt(digitalPinToInterrupt(irPin), countPulse, FALLING);
  }

  // Toggle display between title and RPM value
  if (millis() - displayToggleTime >= toggleInterval) {
    displayToggleTime = millis();
    showTitle = !showTitle;
    
    lcd.clear();
    if (showTitle) {
      // Center "RPM COUNTER" text
      lcd.setCursor(3, 0);
      lcd.print("RPM COUNTER");
    } else {
      // Center RPM value
      lcd.setCursor(4, 0);
      lcd.print(rpm);
      lcd.print(" RPM");
    }
  }
}

// Interrupt service routine to count pulses
void countPulse() {
  pulseCount++;
}

Press on verify and then press upload so the code is transferred to your arduino UNO 
