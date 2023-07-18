#define BLYNK_TEMPLATE_ID "TMPL6tTyQK4NB"
#define BLYNK_TEMPLATE_NAME "tnh"
#define BLYNK_AUTH_TOKEN "HscJjzT8zah1NtPgvyza-X3JcgJHUNhZ"

#define BLYNK_PRINT Serial
#include <WiFi.h>
#include <BlynkSimpleEsp32.h>
#include <DHT.h>



char auth[] = BLYNK_AUTH_TOKEN;

char ssid[] = "Afroto-TIME 2.4Ghz";  // type your wifi name
char pass[] = "AB_@@1971@@";  // type your wifi password

BlynkTimer timer;


#define DHTPIN 27 //Connect Out pin to D2 in NODE MCU
#define DHTTYPE DHT11  
DHT dht(DHTPIN, DHTTYPE);


void sendSensor()
{
  float h = dht.readHumidity();
  float t = dht.readTemperature(); // (true) Fahrenheit

  if (isnan(h) || isnan(t)) {
    Serial.println("Failed to read from DHT sensor!");
    return;
  }
  // You can send any value at any time.
  // Please don't send more that 10 values per second.
    Blynk.virtualWrite(V0, t);
    Blynk.virtualWrite(V1, h);
    Serial.print("Temperature : ");
    Serial.print(t);
    Serial.print("    Humidity : ");
    Serial.println(h);
    //delay(500)
}
void setup()
{   
  
  Serial.begin(115200);
  

  Blynk.begin(auth, ssid, pass);
  dht.begin();
  timer.setInterval(100L, sendSensor);
 
  }

void loop()
{
  Blynk.run();
  timer.run();
}
