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


## Ejercicio 2
  ### Resumen del Concepto de Hilos
- Un hilo es la unidad más pequeña de procesamiento que puede ejecutar código de forma independiente. En Unity, los hilos nos permiten ejecutar operaciones en paralelo, fuera del hilo principal, que es donde ocurre la ejecución de la lógica del juego y la renderización de gráficos. Este enfoque evita que tareas que puedan tomar tiempo (como leer datos de un puerto serial) bloqueen la ejecución de otras tareas.
  
### Importancia del Uso de Hilos en Ardity
- En el contexto de Ardity, el plugin para comunicación serial, los hilos son esenciales porque permiten que el flujo de datos seriales entre Arduino y Unity ocurra de manera continua y sin interrupciones para la experiencia del usuario. Al delegar la lectura y escritura de datos a un hilo secundario, Unity puede procesar otros aspectos del juego en el hilo principal sin verse afectado por posibles demoras o bloqueos en la comunicación serial.
  
## Ejemplo de Flujo de Comunicación Serial en Ardity

### Inicio del Hilo Secundario:

'SerialController' inicia el hilo al crear una instancia de 'SerialThread' y llamando a 'Thread.Start()'.
Lectura Continua del Puerto Serial:

'SerialThread' ejecuta un bucle en el hilo secundario que lee datos del puerto serial y los coloca en una cola.
Este bucle usa 'Thread.Sleep()' para no saturar la CPU mientras espera nuevos datos.
Recuperación de Datos en el Hilo Principal:

'SerialController' utiliza 'ReadSerialMessage()' en el hilo principal de Unity para revisar si hay mensajes nuevos en la cola que el hilo secundario llenó. Si hay mensajes, los procesa y ejecuta las funciones correspondientes en Unity.
Envío de Datos:

Si se requiere enviar datos al dispositivo conectado, 'SerialController' utiliza 'SendSerialMessage()' para pasar un mensaje al 'SerialThread', que lo escribe en el puerto serial.


## Análisis del Plugin Ardity - Ejercicio 3
Este análisis se centra en la implementación de comunicación serial entre Unity y Arduino utilizando el plugin Ardity. Revisaremos el funcionamiento del código de Arduino y su integración en Unity, utilizando como referencia la escena DemoScene_UserPoll_ReadWrite.

Código Arduino
A continuación, se presenta el código de Arduino usado en este demo para un protocolo ASCII:

    uint32_t last_time = 0;

    void setup() {
    Serial.begin(19200);
    }

    void loop() {
    // Enviar "Arduino is alive!!" cada 5 segundos
    if ((millis() - last_time) > 5000) {
        Serial.print("Arduino is alive!!\n");
        last_time = millis();
    }

    // Enviar un mensaje dependiendo del carácter recibido ('A' o 'Z')
    switch (Serial.read()) {
        case 'A':
            Serial.print("That's the first letter of the abecedarium.\n");
            break;
        case 'Z':
            Serial.print("That's the last letter of the abecedarium.\n");
            break;
    }
}
### Consideraciones
Velocidad de Comunicación: La velocidad de comunicación está configurada en 19200 baudios, por lo que Unity debe usar la misma velocidad para que la conexión funcione correctamente.

Uso de millis() en lugar de delay(): millis() mide el tiempo transcurrido sin bloquear el ciclo loop(), permitiendo enviar el mensaje "Arduino is alive!!" cada 5 segundos sin interrumpir la ejecución de otros procesos.

Lectura Directa de Serial.read(): En lugar de usar Serial.available(), el código lee directamente del buffer serial. Si no hay datos en el buffer, Serial.read() devuelve -1, lo cual se ignora aquí, pero es importante tener en cuenta para evitar errores en otras implementaciones.

### Protocolo ASCII: El uso de Serial.print() con \n al final de cada mensaje sugiere un protocolo ASCII de comunicación.

### Análisis del Lado de Unity - DemoScene_UserPoll_ReadWrite
Para analizar esta parte, cargué la escena DemoScene_UserPoll_ReadWrite en Unity. En ella, el GameObject SampleUserPolling_ReadWrite tiene dos componentes:

### Transform: Propio de Unity para posicionamiento.
Script SampleUserPolling_ReadWrite: Contiene el código de interacción entre Unity y Arduino.
El script en este objeto expone la variable pública serialController, que es de tipo SerialController. Esta variable permite la referencia a otro GameObject llamado SerialController. En el editor de Unity, arrastré este GameObject al campo serialController en SampleUserPolling_ReadWrite para enlazar los dos objetos.

### Script SampleUserPolling_ReadWrite
A continuación se muestra el código del script SampleUserPolling_ReadWrite en Unity:


    using UnityEngine;
    using System.Collections;
    
    public class SampleUserPolling_ReadWrite : MonoBehaviour {
    public SerialController serialController;

    // Inicialización
    void Start() {
        serialController = GameObject.Find("SerialController").GetComponent<SerialController>();
        Debug.Log("Press A or Z to execute some actions");
    }

    // Ejecutado en cada frame
    void Update() {
        // Enviar datos
        if (Input.GetKeyDown(KeyCode.A)) {
            Debug.Log("Sending A");
            serialController.SendSerialMessage("A");
        }
        if (Input.GetKeyDown(KeyCode.Z)) {
            Debug.Log("Sending Z");
            serialController.SendSerialMessage("Z");
        }

        // Recibir datos
        string message = serialController.ReadSerialMessage();
        if (message == null) return;

        // Verificar tipo de mensaje recibido
        if (ReferenceEquals(message, SerialController.SERIAL_DEVICE_CONNECTED)) {
            Debug.Log("Connection established");
        } else if (ReferenceEquals(message, SerialController.SERIAL_DEVICE_DISCONNECTED)) {
            Debug.Log("Connection attempt failed or disconnection detected");
        } else {
            Debug.Log("Message arrived: " + message);
        }
    }
    }
Prueba de Ejecución
Configuración del Puerto Serial: Configuré el puerto serial (COM4 en mi caso) en el inspector de Unity para el SerialController.

Prueba de Envío y Recepción:

Abrí la consola y ejecuté la escena en el modo "Game".
Presioné las teclas A y Z para enviar los mensajes correspondientes al Arduino.
Observé los mensajes de respuesta en la consola de Unity, confirmando la recepción de los mensajes enviados por Arduino.
Verificación de la Configuración de SerialController:

Comenté la línea serialController = GameObject.Find("SerialController").GetComponent<SerialController>(); y verifiqué que la comunicación fallaba, lo cual demuestra que esta línea asigna correctamente la referencia del SerialController.
Eliminé la referencia de SerialController en el editor, y la comunicación también falló, indicando que ambos pasos son esenciales para la correcta comunicación serial.
Preguntas Clave
Frecuencia de Envío y Recepción:

La letra "A" o "Z" se envía solo cuando el usuario presiona las teclas correspondientes.
La recepción de mensajes se verifica en cada frame en el método Update().
Detalles de la Comunicación y la Concurrencia:

SerialController usa un segundo hilo (thread) para la comunicación serial:

### protected Thread thread;
protected SerialThreadLines serialThread;
El método onEnable en SerialController inicializa un hilo que ejecuta serialThread.RunForever para mantener una conexión constante con Arduino:

serialThread = new SerialThreadLines(portName, baudRate, reconnectionDelay, maxUnreadMessages);
thread = new Thread(new ThreadStart(serialThread.RunForever));
thread.Start();

## Conclusión
La implementación en DemoScene_UserPoll_ReadWrite permite el envío y recepción de mensajes en tiempo real entre Unity y Arduino. La comunicación es manejada en segundo plano mediante SerialController, lo cual optimiza el flujo sin bloquear la ejecución principal de Unity.
