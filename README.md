#include <elapsedMillis.h>

// Define the analog pin to which the LM35 sensor is connected
const int lm35Pin = A0;

// Define the onboard LED pin
const int ledPin = 13;

// Temperature thresholds
const float lowTempThreshold = 30.0;
const float highTempThreshold = 30.0;

// Blink intervals in milliseconds
const unsigned long lowTempBlinkInterval = 250;
const unsigned long highTempBlinkInterval = 500;

// Enumerate possible states
enum State {
  LOW_TEMP,
  HIGH_TEMP
};

// Initialize the state
State currentState = LOW_TEMP;

// Create an elapsedMillis object to track time
elapsedMillis elapsedTime;

void setup() {
  pinMode(ledPin, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  // Read temperature from the LM35 sensor
  int temperature = analogRead(lm35Pin);
  float celsius = (temperature * 0.0048828125) * 100.0;

  // Check temperature conditions and update state
  if (celsius < lowTempThreshold) {
    currentState = LOW_TEMP;
  } else if (celsius >= highTempThreshold) {
    currentState = HIGH_TEMP;
  }

  // Call the appropriate function based on the current state
  if (currentState == LOW_TEMP) {
    blinkLED(lowTempBlinkInterval);
  } else {
    blinkLED(highTempBlinkInterval);
  }
}

void blinkLED(unsigned long interval) {
  // Check if the specified interval has passed since the last LED update
  if (elapsedTime >= interval) {
    // Toggle the LED state
    digitalWrite(ledPin, !digitalRead(ledPin));

    // Reset the elapsed time for the next interval
    elapsedTime = 0;
  }
}
