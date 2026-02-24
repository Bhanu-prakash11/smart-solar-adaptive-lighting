
*** SMART SOLAR ADAPTIVE LIGHTING SYSTEM ***
-----------------------------------------------------------------------------------------------

GOAL : 
-> When there is DARK : (LDR)
	-- if MOTION is DETECTED (PIR) then the LIGHT should GLOW with 100% EFFICIENCY
	-- if MOTION is NOT DETECTED (PIR) then the LIGHT should GLOW with 50% EFFICIENCY
-> Wen there is LIGHT :
	-- Regardless of MOTION DETECTION ; LIGHT should be OFF 
-----------------------------------------------------------------------------------------------

FINAL WORKING CODE :

const int ldrPin = A0;        // LDR AO pin connected to A0
const int pirPin = 2;         // PIR sensor OUT pin
const int mosfetPin = 9;      // PWM pin connected to IRFZ44N gate

int ldrValue = 0;
int pirValue = 0;

void setup() {
  pinMode(pirPin, INPUT);
  pinMode(mosfetPin, OUTPUT);
  Serial.begin(9600); // Optional for debugging
}

void loop() {
  ldrValue = analogRead(ldrPin);       // Read light level
  pirValue = digitalRead(pirPin);      // Read motion status

  Serial.print("LDR Value: ");
  Serial.print(ldrValue);
  Serial.print(" | PIR: ");
  Serial.println(pirValue);

  // Adjust this threshold based on your actual lighting condition
  if (ldrValue > 500) { // DARK condition (change threshold if needed)
    if (pirValue == HIGH) {
      analogWrite(mosfetPin, 255); // Motion + dark → full brightness
    } else {
      analogWrite(mosfetPin, 128); // Dark only → half brightness
    }
  } else {
    analogWrite(mosfetPin, 0); // Daylight → LED OFF
  }

  delay(100); // Short delay
}
---------
OUTPUT :
 
When LDR > 500 && PIR > 0 → LIGHT GLOWS at 100% EFFICIENCY
When LDR > 500 && PIR = 0 → LIGHT GLOWS at 50% EFFICIENCY
When LDR < 500 && PIR >= 0 → LIGHT OFF

------------------------------------------------------------------------------

