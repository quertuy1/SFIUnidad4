# SFI Unidad4

### **Introducción**

Llegaste al final del curso. En esta unidad aplicarás todos los conceptos que has aprendido para la construcción de aplicaciones interactivas que integren sistemas embebidos con plataformas de cómputo interactivas.

### **Propósito de aprendizaje**

Aplicar los conceptos y procedimientos aprendidos en el curso para resolver un problema que requiera la integración de una plataforma de tiempo real con un sistema microcontrolado.

## Ejercicio 1
Para el proyecto con el plugin Ardity, aquí tienes un modelo para documentar en el archivo README.md. Este archivo incluye pasos generales para instalar, ejecutar y explorar el demo, seguido de la estructura de escenas y una introducción al análisis que harás en el futuro.

### Ardity Plugin - Implementación del Demo y Análisis de Escenas
Este documento detalla el proceso de implementación y análisis del demo del plugin Ardity, el cual permite la comunicación entre Unity y Arduino. A continuación, expongo los pasos seguidos para poner en marcha el demo, junto con el análisis de las escenas incluidas en el proyecto.

### 1. Requisitos y Preparación
Para este ejercicio, estoy utilizando:

Unity 2018.2: Esta versión es la recomendada para el plugin Ardity.
Arduino: El plugin está diseñado para interactuar con un microcontrolador Arduino.
Ardity Plugin: Disponible en este repositorio.
### 2. Instalación y Configuración
Descarga del Plugin: Comencé clonando el repositorio de Ardity en mi equipo con el siguiente comando:
bash
Copiar código
git clone https://github.com/DWilches/Ardity.git
Apertura en Unity: Abrí el proyecto en Unity 2018.2 y verifiqué que todos los assets y scripts se importaran correctamente.
Revisión del README Original: Consulté el archivo README.md del repositorio para obtener una visión general sobre la configuración y funcionamiento básico del plugin.
3. Ejecución y Exploración de Escenas de Ejemplo
En la carpeta Scenes, encontré cinco escenas de demo que probé para familiarizarme con el flujo de comunicación entre Unity y Arduino:

- DemoScene 1: Configuración básica de conexión.
- DemoScene 2: Ejemplo de envío de datos desde Arduino hacia Unity.
- DemoScene 3: Comunicación de datos en dirección de Unity a Arduino.
- DemoScene 4: Ejemplo de flujo bidireccional.
- DemoScene_UserPoll_ReadWrite: Escena avanzada que explora la lectura y escritura de datos en tiempo real.
Para cada una de estas escenas, ejecuté el demo en Unity, observando el flujo de datos y las configuraciones de cada uno de los scripts involucrados. Esto me permitió entender cómo se establece y gestiona la conexión entre Unity y Arduino en diferentes escenarios de comunicación.

### 4. Análisis Detallado: DemoScene_UserPoll_ReadWrite
Elegí la escena DemoScene_UserPoll_ReadWrite para un análisis en profundidad debido a su configuración avanzada de comunicación en tiempo real. En esta escena, el flujo de datos es bidireccional, permitiendo monitorear y escribir datos de forma continua entre Arduino y Unity. A continuación, detallo los elementos clave que observé:

Flujo de Comunicación: La escena utiliza un sistema de polling que permite mantener la conexión abierta para enviar y recibir datos de manera eficiente.
Scripts Involucrados: Me enfoqué en los scripts que gestionan tanto la lectura como la escritura en el puerto serial. Observé cómo se manejan los eventos de transmisión de datos, los métodos de actualización, y la respuesta en tiempo real de los datos entre Unity y Arduino.
Este análisis me proporcionó una comprensión profunda sobre cómo el plugin Ardity facilita la comunicación bidireccional en un contexto más complejo.

### 5. Próximos Pasos y Conclusiones
A partir de este análisis, me dispongo a:

Identificar oportunidades de personalización en la escena DemoScene_UserPoll_ReadWrite para adaptarla a casos de uso específicos.

Documentar cualquier ajuste adicional y probar diferentes configuraciones para optimizar el flujo de datos.

### Recursos

- Repositorio oficial de Ardity: GitHub - Ardity
- Documentación de Unity Serial Communication: Unity Serial Communication Docs
