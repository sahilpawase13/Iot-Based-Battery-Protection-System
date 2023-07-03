`//IOT Based Battery Protection System...

#include <Wire.h> 
#include <LiquidCrystal_I2C.h>

LiquidCrystal_I2C lcd(0x27,20,4); 

int buzz = 13;
int relay = 12;
int gasPin = A0; 
int voltagePin = A1; 
int tempPin = A2; 

void setup() 
{
  Serial.begin(9600);
  lcd.init();
  lcd.backlight();
  pinMode(relay,OUTPUT);
  pinMode(buzz,OUTPUT);
  pinMode(voltagePin,INPUT);
  pinMode(gasPin, INPUT);
  pinMode(tempPin,INPUT);
  digitalWrite(relay,HIGH);

}

void alert()
{
  int i = 0;
  for(i;i<20;i++)
  {
    digitalWrite(buzz,HIGH);
    delay(100);
    digitalWrite(buzz,LOW);
    delay(100);
  }
}

void loop() {
  int gasValue = analogRead(gasPin); // Read the analog value from the gas sensor
  float voltageValue = analogRead(voltagePin) * (5.0 / 1023.0); // Convert the analog value to voltage (assuming 5V Arduino board)
  float tempValue = (analogRead(tempPin) * 5.0 / 1023.0) * 100.0; // Convert the analog value to temperature in Celsius



  Serial.print("Gas Concentration: ");
  Serial.print(gasValue);
  Serial.println("%");
  
  Serial.print("Voltage: ");
  Serial.print(voltageValue);
  Serial.println("V");

  Serial.print("Temperature: ");
  Serial.print(tempValue);
  Serial.println("°C");

  lcd.clear(); // Clear the LCD display
  
  lcd.setCursor(0, 0); // Set the cursor to the first line
  lcd.print("Gas: ");
  if(gasValue>520)
  {
  
      digitalWrite(relay,HIGH);
      lcd.print("Abnormal");
      lcd.setCursor(1,1);
      lcd.print("Gas Leaked");
      alert();
      
    
      
  }
  else
  {
    lcd.print("Normal");
    digitalWrite(relay,LOW);
  }
  delay(2000);

  lcd.setCursor(0,1);
  if(voltageValue>=12)
  {
    digitalWrite(relay,HIGH);
    lcd.clear();
    lcd.setCursor(0, 0); // Set the cursor to the second line
    lcd.print("Voltage: ");
    lcd.print(voltageValue);
    
    lcd.setCursor(1,1);
    lcd.print("_OverCharged_");
    alert();
    delay(5000);
    lcd.clear();
  }
  else
  {
    lcd.setCursor(0, 1); // Set the cursor to the second line
    lcd.print("Voltage: ");
    lcd.print(voltageValue);
    lcd.print("v");
    digitalWrite(relay,LOW);
  }
  delay(2000);

  lcd.clear();
  lcd.setCursor(1,0);
  if(tempValue>45)
  {
    digitalWrite(relay,HIGH);
    lcd.print("Battery Heat Up..");
    alert();
  }
  else
  {
    digitalWrite(relay,LOW);

  lcd.print("Temp= :");
  lcd.print(tempValue);
  lcd.print("C");
  }
  
  delay(2000);
  lcd.clear();
  lcd.setCursor(0,0);
  lcd.print("_The Incognito_");
  lcd.setCursor(2,1);
  lcd.print("__Thank You__");
  delay(1000); // Delay for 1 second before taking the next reading
  
  
}

![Actual Image](C:\Users\SNEHAL\Desktop\IMG_20230619_112817.jpg)`