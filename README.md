#include "WiFi.h"
#include "DHTesp.h"
#include "HTTPClient.h"

const char* ssid = "Wokwi-GUEST";
const char* password = "";
const String url = "https://api.thingspeak.com/update?api_key=0JBJT2DZ1R3YMCAM&";

HTTPClient h;
DHTesp dhtSensor;

void setup() {
  
  Serial.begin(115200);
  Serial.println("Iniciando Setup");
  dhtSensor.setup(13, DHTesp::DHT22);

  WiFi.begin(ssid, password);

  while(WiFi.status() != WL_CONNECTED){
    delay(500);
    Serial.println("Conecting to WiFi...");
  
  }
  pinMode(14, OUTPUT);
  pinMode(23, OUTPUT);
  Serial.println("Finalizando Setup");

}

void loop() {
  //conectado 
  float temperatura = 24.00;
  float umidade = 55.50;

  String path = url + "field1=" + String(temperatura) + "&field2=" + String(umidade);
  h.begin(path);
  int httpCode = h.GET();
  String payload = h.getString();
  Serial.println("HTTPCode:" + String(httpCode));
  Serial.println("Payload:" + payload);


  delay(1000);
  //capturando temperatura e humidade
  float temperature = dhtSensor.getTemperature();
  float humidade = dhtSensor.getHumidity(); 

  //imprimindo dados capturados
  Serial.println("Temperatura :" + String(temperatura) + "°C");
  Serial.println("Humidade :" + String(humidade) + "%");

  delay(1000);

  digitalWrite(14, HIGH);
  digitalWrite(23, HIGH);
  delay(1000);

  digitalWrite(14, LOW);
  digitalWrite(23, LOW);
  delay(1000);
}
