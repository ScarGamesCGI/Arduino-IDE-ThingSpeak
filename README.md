# Arduino-IDE-ThingSpeak
Aqui encontaras todo lo necesario para el codigo de la pagina de HMTL y arduino.

#Codigo de Arduino.
#Incluye las librerías necesarias para conectarse a WiFi y a la plataforma ThingSpeak.
#include &lt;ThingSpeak.h&gt;

#Se definen las credenciales de red WiFi y el cliente para conectar a ThingSpeak.
const char* ssid = "TU_SSID";              // ⚠️ Reemplaza con tu SSID real
const char* password = "TU_PASSWORD";      // ⚠️ Reemplaza con tu contraseña real
WiFiClient client;

#Se configuran los datos del canal de ThingSpeak: ID del canal y clave para enviar datos (Write API Key).
unsigned long channelID = 123456;          // ⚠️ Reemplaza con tu Channel ID real
const char* writeAPIKey = "TU_API_KEY";    // ⚠️ Reemplaza con tu Write API Key real

void setup() {
  Serial.begin(115200);
  WiFi.begin(ssid, password);
  Serial.print("Conectando a WiFi");
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  Serial.println("\nWiFi conectado");
  ThingSpeak.begin(client);
}

void loop() {
  if (WiFi.status() != WL_CONNECTED) {
    Serial.println("WiFi desconectado. Reintentando...");
    WiFi.begin(ssid, password);
    while (WiFi.status() != WL_CONNECTED) {
      delay(500);
      Serial.print(".");
    }
    Serial.println("\nWiFi reconectado");
  }

  int valor = analogRead(34);
  Serial.print("Valor leído: ");
  Serial.println(valor);

  int x = ThingSpeak.writeField(channelID, 1, valor, writeAPIKey);

  if (x == 200) {
    Serial.println("Dato enviado correctamente.");
  } else {
    Serial.print("Error al enviar (código ");
    Serial.print(x);
    Serial.println(").");
  }

  delay(15000);
}
