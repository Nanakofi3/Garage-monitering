#include <ESP8266WiFi.h>
#include <BlynkSimpleEsp8266.h>
#define BLYNK_TEMPLATE_ID "TMPLPdQb0fB7"
#define BLYNK_DEVICE_NAME "Garage Monitering"
char auth[] = "hs3yEbmrKTERPRQkU7YSVp49gIsY3xbW";
char ssid[] = "TECNO CAMON 17P";
char pass[] = "bbo@t7772";

const char* iftttKey = "pevmgv-0UBo4VD6zyIF59dyJO0V_WSj_OwdR8Ed7ix";
const char* eventName = "Garbage_is_full";


const int trigPin = D1;
const int echoPin = D2;
const int distanceThreshold = 5;

BlynkTimer timer;


void setup() {
  Serial.begin(115200);
  Blynk.begin(auth, ssid, pass);
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  timer.setInterval(10000L, checkGarbageCan);
}

void loop() {
  Blynk.run();
  timer.run();
}

void checkGarbageCan() {
  long duration, distance;

  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  duration = pulseIn(echoPin, HIGH);
  distance = duration * 0.034 / 2; // Calculate distance in centimeters

  Serial.print("Distance: ");
  Serial.print(distance);
  Serial.println(" cm");

  Blynk.virtualWrite(V0, distance); // Send mapped distance data to Blynk app

  if (distance <= distanceThreshold) {
    sendIFTTTRequest();
    delay(60000); // Delay to prevent multiple alerts within a minute
  }
}

void sendIFTTTRequest() {
  // Your existing IFTTT request code here
  WiFiClient client;
  if (client.connect("maker.ifttt.com", 80)) {
    String url = "/trigger/" + String(eventName) + "/with/key/" + String(iftttKey);
    Serial.print("Sending HTTP request to IFTTT: ");
    Serial.println(url);
    client.print(String("GET ") + url + " HTTP/1.1\r\n" +
                 "Host: maker.ifttt.com\r\n" +
                 "Connection: close\r\n\r\n");
    client.stop();
  } else {
    Serial.println("Failed to connect to IFTTT");
  }
}
