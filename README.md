# 📻 ESP32 RDS Transmitter
Implementación de transmisión FM con soporte RDS usando un ESP32.  
Este proyecto permite emitir una señal FM y añadir información RDS como el nombre de la emisora, texto dinámico, PI, PTY, etc.

## 🚀 Características
- Transmisión FM estable.
- RDS totalmente funcional.
- Control por I2C mediante el chip **Si4713**.
- Compatible con ESP32 WROOM y WROVER.
- Ejemplo básico incluido.

---

# 🧩 Hardware necesario

| Componente | Descripción |
|-----------|-------------|
| **ESP32** | Cualquier modelo con I2C |
| **Si4713 FM Transmitter** | Recomendado (Adafruit o módulo compatible) |
| Antena | Cable de ~75 cm |

---

# 🔌 Conexiones ESP32 ↔ Si4713

| Si4713 | ESP32 |
|--------|-------|
| SDA    | GPIO 21 |
| SCL    | GPIO 22 |
| RST    | GPIO 4 |
| 3.3V   | 3.3V |
| GND    | GND |

---
📡 Antena

Para mejorar el alcance, conectar un cable de ~75 cm al pin ANT del Si4713.

🛠 Compilación y carga

Instalar ESP32 Board en el Arduino IDE.

Seleccionar ESP32 Dev Module.

Conectar el ESP32 por USB.

Subir el código.

📚 Licencia

MIT License.

🙌 Agradecimientos

Proyecto basado en las librerías oficiales de Adafruit y la comunidad open-source.

# 📦 Librerías necesarias

Instalar desde el Gestor de Librerías de Arduino:

Adafruit Si4713 Library
Adafruit BusIO

Codigo de ejemplo:

---

# 📜 Código de ejemplo

```cpp
#include <Wire.h>
#include <Adafruit_Si4713.h>

#define RESET_PIN 4

Adafruit_Si4713 radio = Adafruit_Si4713(&Wire, RESET_PIN);

void setup() {
  Serial.begin(115200);
  Wire.begin();

  if (!radio.begin()) {
    Serial.println("No se detecta el Si4713");
    while (1);
  }

  Serial.println("Si4713 listo!");

  // Frecuencia FM en 100.0 MHz
  radio.tuneFM(10000);

  // Potencia TX
  radio.setTXpower(115, 0);

  // Activar RDS
  radio.beginRDS();
  radio.setRDSstation("ESP32FM ");
  radio.setRDSbuffer("Hola desde ESP32 con RDS!");
}

void loop() {
  // Actualizar texto cada 5 segundos
  radio.setRDSbuffer("RDS funcionando!");
  delay(5000);
}
---
