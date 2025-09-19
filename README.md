🌐 Open IT Doors con ESP32

Este proyecto permite controlar dos puertas usando ESP32 y visualizar las cámaras asociadas, todo desde un Dashboard Web.
Cada ESP32 obtiene su propia IP local, la cual se integra en el panel principal para manejar relés de manera remota.

⚙️ Requisitos
Hardware

2 × ESP32

2 × Módulos Relay

Cables USB tipo C

Fuentes de poder de 5V (o puertos USB de PC)

Cámaras IP para monitoreo de puertas

Software

Arduino IDE

Librerías necesarias:

WiFi.h

WebServer.h

HTTPClient.h

🚀 Instalación y Configuración

ESP32 #1 – Dashboard Principal

Flashea el ESP32 con el código ESP32 Main + Relay 1 + Cámaras.

Abre el Serial Monitor y guarda la IP asignada (la usarás en tu navegador).

ESP32 #2 – Relay Secundario

Flashea el ESP32 con el código ESP32 Relay 2.

Guarda la IP generada.

Configura el Dashboard

Abre el código del ESP32 principal.

Añade la IP del ESP32 secundario en la variable esp32Relay2:

const char* esp32Relay2 = "192.168.3.84"; // IP del ESP32 #2


Configura los pines GPIO

Por defecto, Relay 1 → GPIO 2 y Relay 2 → GPIO 26.

Cambia los pines si lo deseas según tu montaje.

🔌 Cableado
ESP32 Pin	Relay Pin
GPIO 2	IN (Relay 1)
GPIO 26	IN (Relay 2)
5V	VCC
GND	GND

📎 Archivo adjunto: ESP32 & Relay Wiring Diagram

🖥️ Uso

Conecta los ESP32 a la PC o a fuentes de poder de 5V mediante USB tipo C.

Abre la IP del Dashboard en tu navegador.

Controla los relés desde la interfaz web y visualiza las cámaras de las puertas en tiempo real.

📸 Ejemplo de Interfaz

(Aquí puedes agregar capturas del Dashboard y del montaje físico)

Diseño Web:

Dos columnas: puerta principal (Relay 1 + Cámara 1) y cocina (Relay 2 + Cámara 2).

Botones “Click to Open” para cada puerta.

Auto-refresh de cámaras cada segundo.

🔄 Flujo de Control
flowchart LR
    A[Usuario abre Dashboard ESP32 #1] --> B{Elige puerta}
    B -->|Relay 1| C[ESP32 #1 activa Relay 1]
    B -->|Relay 2| D[ESP32 #1 envía HTTP a ESP32 #2]
    D --> E[ESP32 #2 activa Relay 2]
    C & E --> F[Cámara actualizada en la web]

📝 Notas

Todos los ESP32 se alimentan con USB tipo C y fuentes de 5V.

Asegúrate de reservar las IPs en DHCP para producción y evitar cambios de IP.

Las cámaras IP deben estar accesibles desde la misma red.

📄 Licencia

Este proyecto es de uso libre para fines educativos y experimentales.
