# Task_4
#define BLYNK_TEMPLATE_ID "TMPL3MPBPPhMq"
#define BLYNK_TEMPLATE_NAME "Smart automation System"
#define BLYNK_AUTH_TOKEN "Q5udjEsyKawQce8qTVs50piVFG_K7Qxb"

#define BLYNK_PRINT Serial

#include <WiFi.h>
#include <BlynkSimpleEsp32.h>
#include <DHT.h>

char ssid[] = "Wokwi-GUEST";
char pass[] = "";

#define DHTPIN 15
#define DHTTYPE DHT22

DHT dht(DHTPIN, DHTTYPE);

BlynkTimer timer;

void sendTemperature()
{
  float temperature = dht.readTemperature();

  if (isnan(temperature))
  {
    Serial.println("Failed to read temperature!");
    return;
  }

  Serial.print("Temperature: ");
  Serial.print(temperature);
  Serial.println(" °C");

  Blynk.virtualWrite(V0, temperature);
}

void setup()
{
  Serial.begin(115200);

  dht.begin();

  Serial.println("Connecting to Blynk...");

  Blynk.begin(BLYNK_AUTH_TOKEN, ssid, pass);

  timer.setInterval(2000L, sendTemperature);
}

void loop()
{
  Blynk.run();
  timer.run();
}
