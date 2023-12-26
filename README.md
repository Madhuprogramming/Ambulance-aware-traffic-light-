# Ambulance-aware-traffic-light-
This repository contains Arduino code and schematics for implementing an "Ambulance Aware Traffic Light System" in a 4-way junction. The system uses binary code transmitted from an ambulance to control the traffic lights and give priority to the ambulance.

Overview:
This project aims to create an intelligent traffic light system that can be triggered by an ambulance using binary signals. The system allows the ambulance to communicate its approach, and the traffic lights respond by providing a clear path for the ambulance.

Components:

Arduino Boards (for both transmitter and receiver)
RF Transmitter and Receiver Modules
LEDs (for traffic lights)
Ambulance Binary Code Transmitter (can be a separate Arduino with buttons or another communication method)

Setup Instructions:

Clone or download this repository.
Connect the components according to the provided schematics.
Upload the Arduino code to the respective boards (transmitter and receiver) using the Arduino IDE.
Set up the Ambulance Binary Code Transmitter to simulate ambulance signals.
Power on the system and observe the traffic light behavior based on the received signals.


CODE:
TX
#include <VirtualWire.h>

const int transmitPin = 12;  // Pin connected to the RF transmitter

void setup() {
  vw_set_tx_pin(transmitPin);
  vw_setup(2000);  // Bits per second
}

void loop() {
  // Simulate an ambulance sending binary code
  int binaryCode = 0b10101010;  // Change this based on your encoding scheme

  vw_send((uint8_t *)&binaryCode, sizeof(binaryCode));
  vw_wait_tx();  // Wait until the transmission is complete

  delay(5000);  // Send the signal every 5 seconds (adjust as needed)
}
RX
#include <VirtualWire.h>

const int receivePin = 11;  // Pin connected to the RF receiver
const int greenLED = 7;     // Pin connected to the green LED
const int redLED = 8;       // Pin connected to the red LED

void setup() {
  vw_set_rx_pin(receivePin);
  vw_setup(2000);  // Bits per second

  pinMode(greenLED, OUTPUT);
  pinMode(redLED, OUTPUT);

  Serial.begin(9600);
}

void loop() {
  uint8_t buf[VW_MAX_MESSAGE_LEN];
  uint8_t buflen = VW_MAX_MESSAGE_LEN;

  if (vw_get_message(buf, &buflen)) {
    int receivedCode = *((int *)buf);

    // Check if the received binary code corresponds to the ambulance signal//
    
    if (received code == 0b10101010) {
      // Ambulance signal received, give priority to the ambulance
      digitalWrite(greenLED, HIGH);
      digitalWrite(redLED, LOW);
      delay(5000);  // Allow the ambulance to pass for 5 seconds (adjust as needed)
      digitalWrite(greenLED, LOW);
      digitalWrite(redLED, HIGH);
    }
  }
}
