### 🌐 Open IT Doors with ESP32

---

Controla dos puertas usando ESP32, donde cada placa controla su propio relé y ambos aparecen en la misma página web.

### ⚙️ Características

- Usa dos placas ESP32

- Cada ESP32 controla su propio relé

- Ambas se visualizan en la misma página web

---

### 🚀 Cómo Funciona

1- ESP32 #1 → Hospeda la página web principal y controla Relay 1

2- ESP32 #2 → Expone un endpoint simple (por ejemplo /pulse) para activar Relay 2

3- La página web principal de ESP32 #1 envía solicitudes a ambos ESP32, mostrando ambos relés y cámaras en un solo lugar

---

###🔌 ESP32 #2 (Relay Only)

- Flashea este código en el segundo ESP32

- Es un servidor pequeño que únicamente controla su relé

Nota:

- "Toma nota de la IP que imprime en el Serial Monitor (ejemplo: 192.168.3.84)"

- "Esta IP será necesaria para el ESP32 principal"

- "Para producción, se recomienda reservar esta IP en el DHCP para que sea permanente"

---

### 💻 ESP32 #1 (Main Web Page + Relay 1 + Cámaras + Llama a ESP32 #2)

- Flashea este código en el ESP32 principal

- Esta placa maneja Relay 1, muestra ambas cámaras y envía solicitudes al ESP32 #2 para Relay 2

---

### 📷 Cámaras

- Las variables cameraURL1 y cameraURL2 corresponden a los feeds de las cámaras de las puertas

- Los feeds son proporcionados por el servidor de cámaras ZoneMinder:

    - Servidor: 192.168.3.241 (IOT.sport.local)

    - App: http://iot/zm/index.php

- Asegúrate de que las cámaras sean accesibles desde la misma red que los ESP32

---

### 📝 Notas Importantes

- Todos los ESP32 deben estar conectados a la misma red WiFi (FFNET)

- Reserva IPs en DHCP para producción y evitar cambios de IP

- Cada relé tiene un pin específico por defecto:

   - Relay 1: GPIO 2

   - Relay 2: GPIO 26

---

###💡 Sugerencias de Uso

1 - Conecta los ESP32 a la PC o a fuentes de poder de 5V mediante USB tipo C

1 - Abre la IP del Dashboard en tu navegador

3 - Controla los relés desde la interfaz web y visualiza las cámaras en tiempo real

---

###📄 Licencia
