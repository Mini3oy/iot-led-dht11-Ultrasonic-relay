# iot-led-dht11-Ultrasonic-relay
#define BLYNK_PRINT Serial
#include <WiFi.h>
#include <WiFiClient.h>
#include <BlynkSimpleEsp32.h> // 

#include "DHT.h"

#define DHTPIN 12 //ขา DHT ที่เชื่อมกับบอร์ด D12
#define DHTTYPE DHT11  
DHT dht(DHTPIN, DHTTYPE);
char auth[] = "Puv7NLn6JQW1g37_oD1Q6vMy_y59QNC7"; //รหัสBLYNK ที่ส่งเข้าเมล

char ssid[] = "3BB_2.4GHz";  //ชื่อไวฟาย
char pass[] = "1111100000"; //รหัสไวไฟ

const int pingPin = 35;
int inPin = 34;

void setup()
{
  Serial.begin(9600);
  
  Blynk.begin(auth, ssid, pass);
  Serial.println();
  Serial.println("Status\tHumidity (%)\tTemperature (C)\t(F)");
  dht.begin();
}

void loop()
{
  float humidity = dht.readHumidity(); // ดึงค่าความชื้น
  float temperature = dht.readTemperature(); // ดึงค่าอุณหภูมิ
  long duration, cm;
  pinMode(pingPin, OUTPUT);
  digitalWrite(pingPin, LOW);
  delayMicroseconds(2);
  digitalWrite(pingPin, HIGH);
  delayMicroseconds(5);
  digitalWrite(pingPin, LOW);
  pinMode(inPin, INPUT);
  duration = pulseIn(inPin, HIGH);
  cm = microsecondsToCentimeters(duration);
  Serial.print(cm);
  Serial.print("cm");
  Serial.println();
  Blynk.run();
  delay(300);
  Blynk.virtualWrite(V0, temperature);
  Blynk.virtualWrite(V1, humidity);
  Blynk.virtualWrite(V2, cm);
} 
  long microsecondsToCentimeters(long microseconds)
{
  return microseconds / 29.1 / 2;
}

