# Reporte
📘 Reporte Técnico del Prototipo
1. Introducción

El presente prototipo implementa un sistema de reconocimiento de objetos en tiempo real empleando visión artificial y señalización multimodal. Utiliza un módulo con capacidades de cómputo embebido para identificar clases de objetos, mostrar la categoría detectada en un display y activar indicadores visuales y acústicos. Su diseño integra una carcasa 3D funcional que alberga la electrónica necesaria para su operación.

2. Objetivo

Desarrollar un prototipo capaz de:

Identificar al menos dos clases de objetos mediante visión artificial.

Mostrar la clase detectada en un display integrado.

Activar señalización mediante LEDs y buzzer según la detección.

Integrar completamente la electrónica en una carcasa impresa en 3D.

Todo esto asegurando un funcionamiento estable y evidencias experimentales documentadas.

3. Materiales

Electrónica

Módulo microcontrolador con cámara (por ejemplo ESP32-CAM)

LEDs (al menos: Naranja, amarillo, rojo u otros según las clases)

Buzzer piezoeléctrico

Display (OLED o TFT, según implementación)

Cables tipo jumper

Fuente de alimentación o módulo regulador

Estructura física

Carcasa impresa en 3D

Software

Entorno de programación 

Librerías para cámara, display y control de pines

Modelo de clasificación de objetos

Scripts de prueba y documentación

4. Desarrollo
4.1 Electrónica

El sistema se construyó integrando los siguientes elementos:

Cámara integrada para capturar imágenes en tiempo real.

Display conectado mediante interfaz serial (I2C/SPI según el módulo).

LEDs conectados a pines GPIO para indicar el tipo de objeto reconocido.

Buzzer utilizado para emitir una señal acústica durante la detección.

Toda la electrónica fue ensamblada en una carcasa impresa en 3D, permitiendo ventilación y acceso visual a la cámara.

Los diagramas de conexión se documentaron mediante herramientas como Fritzing

4.2 Software

El software incluye:

Carga del modelo de clasificación previamente entrenado.

Captura continua de imágenes para inferencia.

Procesamiento de predicciones para determinar la clase del objeto.

Actualización del display con la etiqueta correspondiente.

Activación de LEDs y buzzer según el resultado.

Registro de evidencias de funcionamiento.


5. Resultados

El prototipo logró:

Identificar correctamente al menos dos clases de objetos en condiciones reales.

Mostrar de manera clara el nombre de la clase en el display.

Activar el LED correspondiente a cada clase detectada.

Emitir una señal acústica coherente con la detección.

Integrar todo el sistema en una carcasa 3D estable y funcional.

Las pruebas incluyen registros fotográficos y de video, demostrando un funcionamiento fiable.

6. Conclusión

El desarrollo del prototipo permitió validar la operación conjunta de visión artificial, señalización electrónica y diseño mecánico. La solución demuestra que es posible construir sistemas compactos capaces de realizar reconocimiento en tiempo real con componentes económicos. La implementación física, así como la claridad del código y de la documentación, contribuyen a su potencial uso académico o como plataforma base para aplicaciones más avanzadas.

7. Trabajos Futuros

Entre las mejoras potenciales se encuentran:

Incrementar el número de clases reconocidas.

Entrenar un modelo propio con mayor precisión.

Implementar comunicación inalámbrica (WiFi/Bluetooth) para enviar resultados.

Desarrollar una interfaz web para monitoreo en tiempo real.

Optimizar la carcasa para mejor manejo térmico.

📚 Fuentes Bibliográficas
[1] A. Rosebrock, Deep Learning for Computer Vision, PyImageSearch, 2019.
[2] F. Pérez y M. Álvarez, Procesamiento Digital de Imágenes, 3ra ed., Madrid, España: Alfaomega, 2020.
[3] A. García y L. Torres, “Introducción a los sistemas embebidos y sus aplicaciones,” Revista Iberoamericana de Ingeniería, vol. 15, no. 2, pp. 45–58, 2021.
[4] J. R. Martínez, Fundamentos de Electrónica Digital, Barcelona, España: Marcombo, 2018.
[5] S. López y R. Hernández, “Aplicaciones de la visión por computadora en prototipos de bajo costo,” Revista Tecnológica del Sur, vol. 12, no. 1, pp. 30–39, 2022.