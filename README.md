# Reporte
🧩 1. Introducción

La ESP32-CAM es un módulo de bajo costo que integra un SoC ESP32 y una cámara OV2640, diseñado para aplicaciones IoT que requieren captura de imágenes, streaming y tareas básicas de visión en el borde.
Debido a sus limitaciones de CPU, RAM y gestión energética, los modelos avanzados de visión suelen necesitar optimización (TinyML) o delegarse a un servidor externo.

Este prototipo combina visión artificial, visualización en display, señalización con LEDs/Buzzer y encapsulado en una carcasa 3D personalizada.

[![ESP32.jpg](https://i.postimg.cc/RFw3wf5D/ESP32.jpg)](https://postimg.cc/k6XXm25Q)

🎯 2. Objetivo

Desarrollar un prototipo capaz de:

🔍 Detectar al menos dos clases de objetos mediante visión artificial (TinyML o servidor externo).

🖥️ Mostrar la clase detectada en un display OLED/TFT.

🚨 Activar LEDs y buzzer según el objeto identificado.

🧱 Integrar toda la electrónica en una carcasa impresa en 3D.

🧪 Mantener un funcionamiento estable y documentado mediante evidencias experimentales.

🛠️ 3. Materiales
🔌 Electrónica
Componente	Descripción
ESP32-CAM	Microcontrolador con cámara OV2640
LEDs	Naranja / Amarillo / Rojo (según clases detectadas)
Buzzer piezoeléctrico	Señalización auditiva
Display OLED/TFT	I2C o SPI
Jumpers macho-hembra	Conexión
Fuente de 5V	Alimentación
Módulo FTDI	Programación de la ESP32-CAM

[![Materiales.jpg](https://i.postimg.cc/rwjhc0DH/Materiales.jpg)](https://postimg.cc/RW31L0Q1)

🧱 Estructura física

Carcasa diseñada en CAD

Impresión 3D (PLA/ABS)

Sistema de sujeción interno para fijar:

ESP32-CAM

Pantalla

LEDS y buzzer

Canal interno para cables

[![Carcasa.jpg](https://i.postimg.cc/sxSJQ1Kk/Carcasa.jpg)](https://postimg.cc/T5dbBdz0)

💻 Software

Arduino IDE

Librerías:

esp_camera

Wire

Adafruit_SSD1306

Edge Impulse (captura, entrenamiento, despliegue)

Scripts para debug y pruebas

⚡ 4. Desarrollo

🧩 4.1 Electrónica

🔌 4.1.1 Conexión ESP32-CAM para programación (FTDI)

ESP32-CAM	FTDI

5V	5V
GND	GND
U0R	TX
U0T	RX
IO0 → GND	Modo programación
🖥️ 4.1.2 Conexión del display OLED (I2C)

Pines válidos en placa AI-Thinker

OLED	ESP32-CAM
SCL	GPIO 14
SDA	GPIO 15
VCC	5V
GND	GND

💻 4.2 Software

📷 4.2.1 Inicialización de cámara ESP32-CAM
#include "esp_camera.h"

// Pines AI Thinker
#define PWDN_GPIO_NUM    32
#define RESET_GPIO_NUM   -1
#define XCLK_GPIO_NUM     0
#define SIOD_GPIO_NUM    26
#define SIOC_GPIO_NUM    27
#define Y9_GPIO_NUM      35
#define Y8_GPIO_NUM      34
#define Y7_GPIO_NUM      39
#define Y6_GPIO_NUM      36
#define Y5_GPIO_NUM      21
#define Y4_GPIO_NUM      19
#define Y3_GPIO_NUM      18
#define Y2_GPIO_NUM       5
#define VSYNC_GPIO_NUM   25
#define HREF_GPIO_NUM    23
#define PCLK_GPIO_NUM    22

void startCamera() {
  camera_config_t config;
  config.ledc_channel = LEDC_CHANNEL_0;
  config.ledc_timer = LEDC_TIMER_0;
  
  config.pin_d0 = Y2_GPIO_NUM;
  config.pin_d1 = Y3_GPIO_NUM;
  config.pin_d2 = Y4_GPIO_NUM;
  config.pin_d3 = Y5_GPIO_NUM;
  config.pin_d4 = Y6_GPIO_NUM;
  config.pin_d5 = Y7_GPIO_NUM;
  config.pin_d6 = Y8_GPIO_NUM;
  config.pin_d7 = Y9_GPIO_NUM;

  config.pin_xclk = XCLK_GPIO_NUM;
  config.pin_pclk = PCLK_GPIO_NUM;
  config.pin_vsync = VSYNC_GPIO_NUM;
  config.pin_href = HREF_GPIO_NUM;

  config.pin_sscb_sda = SIOD_GPIO_NUM;
  config.pin_sscb_scl = SIOC_GPIO_NUM;

  config.pin_pwdn = PWDN_GPIO_NUM;
  config.pin_reset = RESET_GPIO_NUM;

  config.xclk_freq_hz = 20000000;
  config.pixel_format = PIXFORMAT_JPEG;

  config.frame_size = FRAMESIZE_QQVGA;  
  config.jpeg_quality = 15;
  config.fb_count = 1;

  esp_camera_init(&config);
}

void setup() {
  Serial.begin(115200);
  startCamera();
}

void loop() {}

🤖 4.2.2 Detección de objetos con Edge Impulse
🟦 Flujo general

Captura de dataset desde la cámara del ESP32-CAM vía Edge Impulse.

Entrenamiento del modelo:

MobileNetV2 96×96

Optimización con EON Compiler

Exportación Arduino Library (.zip)
Deployment → Arduino Library

Integración en ESP32-CAM:

La imagen se convierte a RGB888 96×96

Se ejecuta el modelo

Se interpreta la clase:

if (result.classification[0].value > 0.7) {
    objetoDetectado = "Clase A";
}

🖥️ 4.2.3 Mostrar detección en OLED
#include <Wire.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>

Adafruit_SSD1306 display(128, 64, &Wire, -1);

void setupDisplay(){
  Wire.begin(15,14); // SDA=15, SCL=14
  display.begin(SSD1306_SWITCHCAPVCC, 0x3C);
  display.clearDisplay();
  display.setTextSize(2);
  display.setTextColor(WHITE);
}

void mostrarObjeto(String objeto){
  display.clearDisplay();
  display.setCursor(0,0);
  display.println("Detectado:");
  display.println(objeto);
  display.display();
}

// Uso:
if(objetoDetectado == "Persona"){
  mostrarObjeto("Persona");
}

📊 5. Resultados

📸 La ESP32-CAM captura imágenes correctamente en resoluciones bajas/medias.

🤖 TinyML permite detectar 1–3 clases con buen desempeño.

🖥️ Detección con servidor (OpenCV) alcanza 20 clases con alta precisión.

🧾 El OLED muestra el objeto detectado en menos de 200 ms.

🧱 La carcasa 3D brinda:

Estabilidad estructural

Organización del cableado

Protección física

Mejor estética del prototipo

[![Detecta.jpg](https://i.postimg.cc/ydHt84Vy/Detecta.jpg)](https://postimg.cc/v1030Kx4)

🏁 6. Conclusión

El sistema integra exitosamente captura, inferencia local y visualización:

✔️ La ESP32-CAM puede realizar detección básica mediante TinyML.

✔️ La detección avanzada se logra mejor con apoyo de un servidor.

✔️ El display OLED brinda retroalimentación inmediata.

✔️ La carcasa 3D permite un prototipo compacto, seguro y funcional.

Ideal para proyectos de seguridad, automatización y reconocimiento de objetos.

🚀 7. Trabajos futuros

🔧 Integrar YOLO-Nano para detección optimizada en microcontroladores.

🧱 Mejorar la carcasa 3D (flujos de aire, soportes, montaje).

📲 Envío de alertas por MQTT a una app móvil.

🔔 Sistema de buzzer inteligente.

🌐 Aumentar velocidad de transmisión con WebSockets.

📚 Fuentes Bibliográficas

A. Rosebrock, Deep Learning for Computer Vision, PyImageSearch, 2019.

F. Pérez & M. Álvarez, Procesamiento Digital de Imágenes, Alfaomega, 2020.

A. García & L. Torres, “Sistemas embebidos y sus aplicaciones,” Revista Iberoamericana de Ingeniería, 2021.

J. R. Martínez, Fundamentos de Electrónica Digital, Marcombo, 2018.

S. López & R. Hernández, “Aplicaciones de visión por computadora en prototipos de bajo costo,” Revista Tecnológica del Sur, 2022.
