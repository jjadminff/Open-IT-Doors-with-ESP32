Open IT Doors with ESP32

Controla dos puertas con ESP32 y visualiza cámaras y relés desde una sola página web.

Requisitos

2 placas ESP32

2 relés (uno por ESP32)

Cámaras IP para monitoreo

Red WiFi interna (FFNET)

Arquitectura del Sistema
+--------------------+          +--------------------+
|    ESP32 #1        |          |     ESP32 #2       |
|  (Main Controller) |          |   (Relay Only)     |
|--------------------|          |--------------------|
| - Web Page Host    |          | - Relay Control    |
| - Relay 1          |          | - Endpoint: /pulse |
| - Cameras 1 & 2    |          |                    |
| - Calls ESP32 #2   |          |                    |
+---------+----------+          +----------+---------+
          |                               ^
          | HTTP Requests                 |
          +-------------------------------+
          
Cámaras IP conectadas a la red → feeds mostrados en la web del ESP32 #1


Flujo de control de los relés:

Usuario accede a la web de ESP32 #1.

Presiona “Click to Open” para la puerta deseada.

Relay 1 → controlado localmente.

Relay 2 → ESP32 #1 envía HTTP GET a /pulse de ESP32 #2.

Cada cámara se refresca automáticamente cada segundo para mostrar la imagen actual.

Configuración de ESP32 #2 (Relay Solo)
#include <WiFi.h>
#include <WebServer.h>

#define RELAY_PIN 26
WebServer server(80);

void handlePulse() {
    digitalWrite(RELAY_PIN, HIGH);
    delay(2000);
    digitalWrite(RELAY_PIN, LOW);
    server.send(200, "text/plain", "Relay 2 activated");
}

void setup() {
    Serial.begin(115200);
    pinMode(RELAY_PIN, OUTPUT);
    digitalWrite(RELAY_PIN, LOW);

    WiFi.begin("FFNET", "W4tchTh3G4M3*");
    while (WiFi.status() != WL_CONNECTED) {
        delay(500);
        Serial.print(".");
    }

    Serial.println("\nESP32 Relay 2 Ready, IP: " + WiFi.localIP().toString());
    server.on("/pulse", handlePulse);
    server.begin();
}

void loop() {
    server.handleClient();
}


Nota: Reserva la IP en tu DHCP para que no cambie.

Configuración de ESP32 #1 (Principal + Relay 1 + Cámaras)
#include <WiFi.h>
#include <WebServer.h>
#include <HTTPClient.h>

#define RELAY1_PIN 2
WebServer server(80);

// URLs de cámaras
const char* cameraURL1 = "http://192.168.3.241/cgi-bin/nphzms?monitor=1";
const char* cameraURL2 = "http://192.168.3.241/cgi-bin/nphzms?monitor=27";

// ESP32 #2 endpoint
const char* esp32Relay2 = "http://192.168.3.84/pulse";

void handleRoot() {
    // HTML + CSS + Scripts para mostrar cámaras y botones de control
}

void handlePulse1() {
    digitalWrite(RELAY1_PIN, HIGH);
    delay(2000);
    digitalWrite(RELAY1_PIN, LOW);
    server.sendHeader("Location", "/");
    server.send(303);
}

void handlePulse2() {
    HTTPClient http;
    http.begin(esp32Relay2);
    http.GET();
    http.end();
    server.sendHeader("Location", "/");
    server.send(303);
}

void setup() {
    Serial.begin(115200);
    pinMode(RELAY1_PIN, OUTPUT);
    digitalWrite(RELAY1_PIN, LOW);

    WiFi.begin("FFNET", "W4tchTh3G4M3*");
    while (WiFi.status() != WL_CONNECTED) { delay(500); Serial.print("."); }
    Serial.println("\nWiFi connected! IP: " + WiFi.localIP().toString());

    server.on("/", handleRoot);
    server.on("/pulse1", handlePulse1);
    server.on("/pulse2", handlePulse2);
    server.begin();
}

void loop() {
    server.handleClient();
}

Flujo de Uso
flowchart LR
    A[Usuario abre web ESP32 #1] --> B{Elige puerta}
    B -->|Relay 1| C[ESP32 #1 activa Relay 1]
    B -->|Relay 2| D[ESP32 #1 envía HTTP a ESP32 #2]
    D --> E[ESP32 #2 activa Relay 2]
    C & E --> F[Cámara actualizada en la web]


Las cámaras se refrescan cada segundo para mostrar el estado en tiempo real.

Si quieres, puedo hacer una versión final en Markdown con imágenes, diagramas en PNG y bloques de código listos para GitHub, lista para subir como README.md. Esto le daría un toque profesional y visual, como los repos de IoT que se ven en GitHub.

¿Quieres que haga esa versión final lista para GitHub?

You said:
si
ChatGPT said:

¡Perfecto! 😄
Voy a crear una versión final de README.md lista para GitHub, con estructura profesional, diagramas y bloques de código bien organizados. Esta versión estará lista para copiar y pegar directamente en tu repositorio. Aquí está:

Open IT Doors with ESP32

Controla dos puertas con ESP32 y visualiza cámaras y relés desde una sola página web.

Requisitos

2 placas ESP32

2 relés (uno por ESP32)

Cámaras IP para monitoreo

Red WiFi interna (FFNET)

Arquitectura del Sistema
+--------------------+          +--------------------+
|    ESP32 #1        |          |     ESP32 #2       |
|  (Main Controller) |          |   (Relay Only)     |
|--------------------|          |--------------------|
| - Web Page Host    |          | - Relay Control    |
| - Relay 1          |          | - Endpoint: /pulse |
| - Cameras 1 & 2    |          |                    |
| - Calls ESP32 #2   |          |                    |
+---------+----------+          +----------+---------+
          |                               ^
          | HTTP Requests                 |
          +-------------------------------+
          
Cámaras IP conectadas a la red → feeds mostrados en la web del ESP32 #1


Flujo de control de los relés:

Usuario accede a la web de ESP32 #1.

Presiona “Click to Open” para la puerta deseada.

Relay 1 → controlado localmente.

Relay 2 → ESP32 #1 envía HTTP GET a /pulse de ESP32 #2.

Cada cámara se refresca automáticamente cada segundo para mostrar la imagen actual.

Configuración de ESP32 #2 (Relay Solo)
#include <WiFi.h>
#include <WebServer.h>

#define RELAY_PIN 26
WebServer server(80);

void handlePulse() {
    digitalWrite(RELAY_PIN, HIGH);
    delay(2000);
    digitalWrite(RELAY_PIN, LOW);
    server.send(200, "text/plain", "Relay 2 activated");
}

void setup() {
    Serial.begin(115200);
    pinMode(RELAY_PIN, OUTPUT);
    digitalWrite(RELAY_PIN, LOW);

    WiFi.begin("FFNET", "W4tchTh3G4M3*");
    while (WiFi.status() != WL_CONNECTED) {
        delay(500);
        Serial.print(".");
    }

    Serial.println("\nESP32 Relay 2 Ready, IP: " + WiFi.localIP().toString());
    server.on("/pulse", handlePulse);
    server.begin();
}

void loop() {
    server.handleClient();
}


Nota: Reserva la IP en tu DHCP para que no cambie.

Configuración de ESP32 #1 (Principal + Relay 1 + Cámaras)
#include <WiFi.h>
#include <WebServer.h>
#include <HTTPClient.h>

#define RELAY1_PIN 2
WebServer server(80);

// URLs de cámaras
const char* cameraURL1 = "http://192.168.3.241/cgi-bin/nphzms?monitor=1";
const char* cameraURL2 = "http://192.168.3.241/cgi-bin/nphzms?monitor=27";

// ESP32 #2 endpoint
const char* esp32Relay2 = "http://192.168.3.84/pulse";

void handleRoot() {
    String html = "<html><head><style>";
    html += "body { background-color: black; color: white; font-family: Arial; text-align: center; }";
    html += "h1 { color: #00ffcc; }";
    html += ".grid { display: grid; grid-template-columns: 1fr 1fr; gap: 20px; justify-items: center; align-items: center; }";
    html += "button { background-color: #00ffcc; border: none; color: black; padding: 15px 30px; font-size: 20px; border-radius: 12px; cursor: pointer; box-shadow: 0 4px 6px rgba(0,0,0,0.3); transition: 0.3s; margin-top: 10px; }";
    html += "button:hover { background-color: #00cca3; box-shadow: 0 6px 10px rgba(0,0,0,0.5); }";
    html += "img { border:4px solid #00ffcc; border-radius:12px; max-width: 100%; height: auto; }";
    html += "</style></head><body>";

    html += "<h1>IT Doors</h1>";
    html += "<div class='grid'>";

    // Relay 1 + Cámara 1
    html += "<div><h2>IT Main</h2>";
    html += "<img id='cam1' src='" + String(cameraURL1) + "' width='720'><br>";
    html += "<a href='/pulse1'><button>Click to Open</button></a></div>";

    // Relay 2 + Cámara 2
    html += "<div><h2>IT Kitchen</h2>";
    html += "<img id='cam2' src='" + String(cameraURL2) + "' width='720'><br>";
    html += "<a href='/pulse2'><button>Click to Open</button></a></div>";

    html += "</div>"; // grid

    // Auto-refresh cámaras
    html += "<script>"
            "setInterval(()=>{document.getElementById('cam1').src='" + String(cameraURL1) + "&rs=' + new Date().getTime();},1000);"
            "setInterval(()=>{document.getElementById('cam2').src='" + String(cameraURL2) + "&rs=' + new Date().getTime();},1000);"
            "</script>";

    html += "</body></html>";
    server.send(200, "text/html", html);
}

void handlePulse1() {
    digitalWrite(RELAY1_PIN, HIGH);
    delay(2000);
    digitalWrite(RELAY1_PIN, LOW);
    server.sendHeader("Location", "/");
    server.send(303);
}

void handlePulse2() {
    HTTPClient http;
    http.begin(esp32Relay2);
    http.GET();
    http.end();
    server.sendHeader("Location", "/");
    server.send(303);
}

void setup() {
    Serial.begin(115200);
    pinMode(RELAY1_PIN, OUTPUT);
    digitalWrite(RELAY1_PIN, LOW);

    WiFi.begin("FFNET", "W4tchTh3G4M3*");
    while (WiFi.status() != WL_CONNECTED) { delay(500); Serial.print("."); }
    Serial.println("\nWiFi connected! IP: " + WiFi.localIP().toString());

    server.on("/", handleRoot);
    server.on("/pulse1", handlePulse1);
    server.on("/pulse2", handlePulse2);
    server.begin();
}

void loop() {
    server.handleClient();
}

Flujo de Uso
flowchart LR
    A[Usuario abre web ESP32 #1] --> B{Elige puerta}
    B -->|Relay 1| C[ESP32 #1 activa Relay 1]
    B -->|Relay 2| D[ESP32 #1 envía HTTP a ESP32 #2]
    D --> E[ESP32 #2 activa Relay 2]
    C & E --> F[Cámara actualizada en la web]

Notas Importantes

cameraURL1 y cameraURL2 corresponden a las cámaras IP de las puertas.

El servidor de cámaras está en 192.168.3.241 (IOT.sport.local) usando ZoneMinder.

Reserva IPs en tu DHCP para producción y evitar cambios de IP.
