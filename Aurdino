
#include <ESP8266WiFi.h>

#define TRIG_PIN_1 D1 // Connect TRIG pin of first ultrasonic sensor to D1 #define ECHO_PIN_1 D2 // Connect ECHO pin of first ultrasonic sensor to D2 #define TRIG_PIN_2 D3 // Connect TRIG pin of second ultrasonic sensor to D3 #define ECHO_PIN_2 D4 // Connect ECHO pin of second ultrasonic sensor to D4 #define LED_PIN_1 D5 // Connect first LED to GPIO pin D5 #define LED_PIN_2 D6 // Connect second LED to GPIO pin D6 #define LED_PIN_3 D7 // Connect third LED to GPIO pin D7 #define LED_PIN_4 D8 // Connect fourth LED to GPIO pin D8

const char WiFiPassword[] = ""; // Set no password to enter const char AP_NameChar[] = "S4Tech";

WiFiServer server(80);

void setup() { Serial.begin(115200); pinMode(TRIG_PIN_1, OUTPUT); pinMode(ECHO_PIN_1, INPUT); pinMode(TRIG_PIN_2, OUTPUT); pinMode(ECHO_PIN_2, INPUT); pinMode(LED_PIN_1, OUTPUT); pinMode(LED_PIN_2, OUTPUT); pinMode(LED_PIN_3, OUTPUT); pinMode(LED_PIN_4, OUTPUT);

// Initialize WiFi WiFi.disconnect(); boolean conn = WiFi.softAP(AP_NameChar, WiFiPassword); server.begin(); }

void loop() { long duration_1, distance_1, duration_2, distance_2;

// Trigger the first ultrasonic sensor digitalWrite(TRIG_PIN_1, LOW); delayMicroseconds(2); digitalWrite(TRIG_PIN_1, HIGH); delayMicroseconds(10); digitalWrite(TRIG_PIN_1, LOW);

// Read the duration of the echo pulse from the first sensor duration_1 = pulseIn(ECHO_PIN_1, HIGH);

// Calculate distance from the first sensor distance_1 = duration_1 * 0.034 / 2; // Speed of sound is 340 m/s, divide by 2 for one-way travel

// Trigger the second ultrasonic sensor digitalWrite(TRIG_PIN_2, LOW); delayMicroseconds(2); digitalWrite(TRIG_PIN_2, HIGH); delayMicroseconds(10); digitalWrite(TRIG_PIN_2, LOW);

// Read the duration of the echo pulse from the second sensor duration_2 = pulseIn(ECHO_PIN_2, HIGH);

// Calculate distance from the second sensor distance_2 = duration_2 * 0.034 / 2; // Speed of sound is 340 m/s, divide by 2 for one-way travel

// Print distance from both sensors to serial monitor Serial.print("Distance 1: "); Serial.print(distance_1); Serial.println(" cm"); Serial.print("Distance 2: "); Serial.print(distance_2); Serial.println(" cm");

// Control LEDs based on distance from both sensors if (distance_1 > 30) { // If the object is far from the first sensor, turn off the first two LEDs digitalWrite(LED_PIN_1, LOW); digitalWrite(LED_PIN_2, LOW); } else { // If the object is near the first sensor, dim and glow the first two LEDs int brightness_1 = map(distance_1, 0, 30, 255, 0); // Map distance to LED brightness (inverse relationship) analogWrite(LED_PIN_1, brightness_1); // Set brightness for LED 1 analogWrite(LED_PIN_2, brightness_1); // Set brightness for LED 2 }

if (distance_2 > 30) { // If the object is far from the second sensor, turn off the second two LEDs digitalWrite(LED_PIN_3, LOW); digitalWrite(LED_PIN_4, LOW); } else { // If the object is near the second sensor, dim and glow the second two LEDs int brightness_2 = map(distance_2, 0, 30, 255, 0); // Map distance to LED brightness (inverse relationship) analogWrite(LED_PIN_3, brightness_2); // Set brightness for LED 3 analogWrite(LED_PIN_4, brightness_2); // Set brightness for LED 4 }

// Check if a client has connected WiFiClient client = server.available(); if (client) { // Read the first line of the request String request = client.readStringUntil('\r');

if (request.indexOf("LEDON") > 1) {
  // Turn on LED
  digitalWrite(LED_PIN_1, HIGH);
  digitalWrite(LED_PIN_2, HIGH);
  digitalWrite(LED_PIN_3, HIGH);
  digitalWrite(LED_PIN_4, HIGH);
  // Respond to client
  client.println("HTTP/1.1 200 OK\r\n");
  client.println("LEDs turned ON");
  client.flush();
  Serial.println("LEDs turned ON");
} else if (request.indexOf("LEDOFF") > 1) {
  // Turn off LED
  digitalWrite(LED_PIN_1, LOW);
  digitalWrite(LED_PIN_2, LOW);
  digitalWrite(LED_PIN_3, LOW);
  digitalWrite(LED_PIN_4, LOW);
  // Respond to client
  client.println("HTTP/1.1 200 OK\r\n");
  client.println("LEDs turned OFF");
  client.flush();
  Serial.println("LEDs turned OFF");
}

// Close the connection
client.stop();
}

delay(1000); // Small delay between distance measurements }
