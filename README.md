#include <LiquidCrystal.h>

// Initialize LCD (RS, E, D4, D5, D6, D7)
LiquidCrystal lcd(12, 11, 5, 4, 3, 2);

// Bluetooth module connected to RX/TX pins
// HC-05 TX → Arduino RX (pin 0)
// HC-05 RX → Arduino TX (pin 1)

String incomingText = "";

void setup() {
  // Start LCD
  lcd.begin(16, 2);
  lcd.print("Waiting...");

  // Start Serial for Bluetooth
  Serial.begin(9600); // HC-05 default baud rate
}

void loop() {
  // Check if data is available from Bluetooth
  while (Serial.available()) {
    char c = Serial.read();
    if (c == '\n') { // End of message
      lcd.clear();
      lcd.setCursor(0, 0);
      lcd.print(incomingText); // Print received text
      incomingText = "";       // Reset buffer
    } else {
      incomingText += c; // Build string
    }
  }
}
