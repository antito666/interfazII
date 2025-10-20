# interfazII
##### Introducción a Processing u Arduino para el desarrollo de una interfaz interactiva Humano-Máquina (humacchina) como pieza artística.
1. [Hola Mundo](#ejercicio-n1-hola-mundo-en-arduino) <br>
2. [LED parpadeante] (#ejercicio-n2-led-parpadeante
3. [] (#https://github.com/antito666/interfazII/blob/main/README.md#ejercicio-n3-led-con-pulsador
4. [] (#https://github.com/antito666/interfazII/blob/main/README.md#ejercicio-n4-led-con-potenciometro
5. [] (#https://github.com/antito666/interfazII/blob/main/README.md#ejercicio-n5-semaforo-en-arduino
6. [] (#
7. [] (#
8. [] (#
9. [] (#
10. [] (#
11. [] (#

##### Ejercicio n°1: "¡Hola Mundo!" en Arduino
```js
void setup() {
  Serial.begin(9600); // Inicia la comunicación serie a 9600 bps
  Serial.println("Hola, Mundo!"); // Envía "Hola, Mundo!" al monitor serial
}

void loop() {
  // No es necesario poner nada en el loop para este ejemplo
}
```
<img src="https://raw.githubusercontent.com/antito666/interfazII/refs/heads/main/Hola_Mundo.png"/>

 ##### Ejercicio n°2: LED parpadeante
```js
void setup() {  // Configuración inicial (ej: pines como entrada/salida)
  pinMode(13, OUTPUT);  // Pin 13 como salida
}

void loop() {   // Se repite infinitamente
  digitalWrite(13, HIGH);  // Encender LED
  delay(1000);             // Esperar 1 segundo
  digitalWrite(13, LOW);   // Apagar LED
  delay(1000);             // Esperar 1 segundo
}
```
<img src="https://raw.githubusercontent.com/antito666/interfazII/refs/heads/main/img/Led_parpadeante.png"/>

##### Ejercicio n°3: LED con pulsador
```js
void setup() {
  pinMode(2, INPUT);  // Botón como entrada
  pinMode(13, OUTPUT);
}
void loop() {
  if (digitalRead(2) == HIGH) {  // Si se presiona el botón
    digitalWrite(13, HIGH);
  } else {
    digitalWrite(13, LOW);
  }
}
```
<img src="https://raw.githubusercontent.com/antito666/interfazII/refs/heads/main/Captura%20de%20pantalla%202025-09-29%20095247.png"/>

##### Ejercicio n°4: LED con potenciometro
```js
void setup() {
  pinMode(9, OUTPUT);  // Pin PWM (símbolo ~)
}
void loop() {
  int valor = analogRead(A0);           // Leer potenciómetro (0-1023)
  int brillo = map(valor, 0, 1023, 0, 255);  // Convertir a rango PWM
  analogWrite(9, brillo);               // Ajustar brillo
}
```
<img src="https://raw.githubusercontent.com/antito666/interfazII/refs/heads/main/img/Led_potenciometro.png"/>
<img src="https://raw.githubusercontent.com/antito666/interfazII/refs/heads/main/IMG_20250818_104553_170.webp"/>

##### Ejercicio n°5: Semaforo en Arduino
```js
// C++ code - Semáforo Autos y Peatones

// Definición de pines
int LED_1 = 6;  // Luz roja autos
int LED_2 = 7;  // Luz amarilla autos
int LED_3 = 8;  // Luz verde autos
int LED_4 = 9;  // Luz verde peatones
int LED_5 = 10; // Luz roja peatones

void setup() {
  // Configuramos todos los pines como salida
  pinMode(LED_1, OUTPUT);
  pinMode(LED_2, OUTPUT);
  pinMode(LED_3, OUTPUT);
  pinMode(LED_4, OUTPUT);
  pinMode(LED_5, OUTPUT);
}

void loop() {
  // 🚦 Fase 1: Autos en verde, peatones en rojo
  digitalWrite(LED_1, LOW);   // Rojo autos apagado
  digitalWrite(LED_2, LOW);   // Amarillo autos apagado
  digitalWrite(LED_3, HIGH);  // Verde autos encendido
  digitalWrite(LED_4, LOW);   // Verde peatones apagado
  digitalWrite(LED_5, HIGH);  // Rojo peatones encendido
  delay(5000); // 5 segundos

  // 🚦 Fase 2: Amarillo autos, peatones siguen en rojo
  digitalWrite(LED_3, LOW);   // Verde autos apagado
  digitalWrite(LED_2, HIGH);  // Amarillo autos encendido
  delay(2000); // 2 segundos
  digitalWrite(LED_2, LOW);   // Amarillo autos apagado

  // 🚦 Fase 3: Rojo autos, verde peatones
  digitalWrite(LED_1, HIGH);  // Rojo autos encendido
  digitalWrite(LED_5, LOW);   // Rojo peatones apagado
  digitalWrite(LED_4, HIGH);  // Verde peatones encendido
  delay(5000); // 5 segundos

  // 🚦 Fase 4: Rojo autos, rojo peatones (tiempo intermedio)
  digitalWrite(LED_4, LOW);   // Verde peatones apagado
  digitalWrite(LED_5, HIGH);  // Rojo peatones encendido
  delay(2000); // 2 segundos
}
```
<img src="https://raw.githubusercontent.com/antito666/interfazII/refs/heads/main/Semaforo.png"/>
<img src="https://raw.githubusercontent.com/antito666/interfazII/refs/heads/main/Captura%20de%20pantalla%202025-09-29%20094550.png"/>

##### Ejercicio n°6: Potenciometro Procesamiento Arduino
###### Código Arduino
```js
unsigned int ADCValue;
void setup(){
    Serial.begin(9600);
}

void loop(){

 int val = analogRead(0);
   val = map(val, 0, 300, 0, 255);
    Serial.println(val);
delay(50);
}
```
<img src="https://raw.githubusercontent.com/antito666/interfazII/6a4316d58ec34617a10bbc6f03152f77cf797868/img/arduino_processing.png"/>

###### Código Processing
```js
import processing.serial.*;

Serial myPort;  // Crear objeto de la clase Serial
static String val;    // Datos recibidos desde el puerto serial
int sensorVal = 0;

void setup()
{
  background(0); 
  //fullScreen(P3D);
   size(1080, 720);
   noStroke();
  noFill();
  String portName = "COM3";// Cambia el número (en este caso) para que coincida con el puerto correspondiente conectado a tu Arduino. 

  //myPort = new Serial(this, "/dev/cu.usbmodem1101", 9600);
  myPort = new Serial(this, Serial.list()[0], 9600);

}

void draw()
{
  if ( myPort.available() > 0) {  // Si hay datos disponibles,
  val = myPort.readStringUntil('\n'); 
  try {
   sensorVal = Integer.valueOf(val.trim());
  }
  catch(Exception e) {
  ;
  }
  println(sensorVal); // léelos y guárdalos en vals!
  }  
 //background(0);
  // Escala el valor de mouseX de 0 a 640 a un rango entre 0 y 175
  float c = map(sensorVal, 0, width, 0, 400);
  // Escala el valor de mouseX de 0 a 640 a un rango entre 40 y 300
  float d = map(sensorVal, 0, width, 40,500);
  fill(255, c, 0);
  ellipse(width/2, height/2, d, d);   
}
```
<img src="https://raw.githubusercontent.com/antito666/interfazII/refs/heads/main/arduino_processing.png"/>

##### Ejercicio n°7: Pulsador + Arduino + Processing
###### Código Arduino
```js
int buttonPin = 2;  // Pin del botón
int buttonState = 0;

void setup() {
  pinMode(buttonPin, INPUT_PULLUP); // Botón con resistencia interna
  Serial.begin(9600);
}

void loop() {
  buttonState = digitalRead(buttonPin);

  if (buttonState == HIGH) {   // Botón presionado
    Serial.println(1);        // Enviar un "1" a Processing
    delay(200);               // Evitar rebotes
  }
}
```
###### Código Processing
```js
import processing.serial.*;

Serial myPort;
ArrayList<PVector> circles; 

void setup() {
  size(1920, 1080);
  background(0);
  
  // Ajusta el nombre del puerto según tu Arduino
  println(Serial.list());
  //myPort = new Serial(this, "/dev/cu.usbmodem1101", 9600);
  myPort = new Serial(this, Serial.list()[0], 9600);
  
  circles = new ArrayList<PVector>();
}

void draw() {
  //background(0);
  
  // Dibujar círculos almacenados
  fill(#2A890E);
  //noStroke();
  stroke(0, 0, 0);
  for (PVector c : circles) {
    ellipse(c.x, c.y, 200, 200);
  }
  
  // Revisar si llega algo de Arduino
  if (myPort.available() > 0) {
    String val = myPort.readStringUntil('\n');
    if (val != null) {
      val = trim(val);
      if (val.equals("1")) {
        // Cada vez que se aprieta el botón, agregar un círculo en posición aleatoria
        circles.add(new PVector(random(width), random(height)));
      }
    }
  }
}
```

##### Ejercicio n°8: Arduino + Pulsador + Potenciómetro + Procesamiento
###### Código Arduino
```js
int buttonPin = 2;       // Pin del botón
int potPin = A0;         // Pin del potenciómetro
int buttonState = 0;

void setup() {
  pinMode(buttonPin, INPUT_PULLUP); // Botón con resistencia interna
  Serial.begin(9600);
}

void loop() {
  buttonState = digitalRead(buttonPin);

  if (buttonState == HIGH) {   // Botón presionado
    int potValue = analogRead(potPin);   // 0 - 1023
    Serial.print("BTN,");     // etiqueta para Processing
    Serial.println(potValue); // mando el valor junto con el evento
    delay(200);               // debounce simple
  }
}
```
###### Código Processing
```js
import processing.serial.*;

Serial myPort;
ArrayList<CircleData> circles; 

void setup() {
  size(1200, 720);
  background(0);
  
  // Ajusta el puerto según tu Arduino
  println(Serial.list());
  myPort = new Serial(this, "/dev/cu.usbmodem1101", 9600);
  //myPort = new Serial(this, Serial.list()[0], 9600);
  
  circles = new ArrayList<CircleData>();
}

void draw() {
  //background(0);
  
  // Dibujar todos los círculos guardados
  fill(#F77AC3);
  //noStroke();
  fill(#F77AC3);
  stroke(0, 0, 0);
  for (CircleData c : circles) {
    ellipse(c.x, c.y, c.size, c.size);
  }
  
  // Leer datos de Arduino
  if (myPort.available() > 0) {
    String val = myPort.readStringUntil('\n');
    if (val != null) {
      val = trim(val);
      if (val.startsWith("BTN")) {
        // Extraer el valor del potenciómetro
        String[] parts = split(val, ',');
        if (parts.length == 2) {
          float potVal = float(parts[1]);
          float circleSize = map(potVal, 0, 1023, 10, 100); // tamaño 10-100 px
          circles.add(new CircleData(random(width), random(height), circleSize));
        }
      }
    }
  }
}

// Clase para guardar datos de cada círculo
class CircleData {
  float x, y, size;
  CircleData(float x, float y, float size) {
    this.x = x;
    this.y = y;
    this.size = size;
  }
}
```
##### Ejercicio n°9: Botonera con sonido - Funcion if + else
###### Código Arduino
```js
// --- Configuración de botones ---
const int numButtons = 3;
const int buttonPins[numButtons] = {2, 4, 7};
const int ledButtonPins[numButtons] = {9, 10, 11}; // LEDs botones

// --- Configuración de potenciómetros ---
const int numPots = 2;
const int potPins[numPots] = {A0, A1};
const int ledPotPins[numPots] = {3, 5}; // LEDs PWM

// Variables de estados previos
int lastButtonState[numButtons];
int lastPotValue[numPots];

void setup() {
  Serial.begin(9600);

  // Configurar botones y LEDs
  for (int i = 0; i < numButtons; i++) {
    pinMode(buttonPins[i], INPUT_PULLUP);
    pinMode(ledButtonPins[i], OUTPUT);
    lastButtonState[i] = digitalRead(buttonPins[i]);
  }

  // Configurar LEDs de potenciómetros
  for (int i = 0; i < numPots; i++) {
    pinMode(ledPotPins[i], OUTPUT);
    lastPotValue[i] = analogRead(potPins[i]);
  }
}

void loop() {
  // Leer y enviar botones
  for (int i = 0; i < numButtons; i++) {
    int buttonState = digitalRead(buttonPins[i]);

    // LED se enciende cuando botón está presionado
    digitalWrite(ledButtonPins[i], buttonState == LOW ? HIGH : LOW);

    if (buttonState != lastButtonState[i]) {  // enviar cambios
      Serial.print("B");
      Serial.print(i); 
      Serial.print(":");
      Serial.println(buttonState);
      lastButtonState[i] = buttonState;
    }
  }

  // Leer y enviar potenciómetros
  for (int i = 0; i < numPots; i++) {
    int potValue = analogRead(potPins[i]); // 0–1023
    int pwmValue = potValue / 4;           // 0–255

    // Ajustar LED según valor
    analogWrite(ledPotPins[i], pwmValue);

    if (abs(pwmValue - lastPotValue[i]) > 2) { // evitar ruido
      Serial.print("P");
      Serial.print(i);
      Serial.print(":");
      Serial.println(pwmValue);
      lastPotValue[i] = pwmValue;
    }
  }

  delay(10);
}
```
<img src="https://raw.githubusercontent.com/antito666/interfazII/refs/heads/main/botonera_arduino_processing.png"/>

###### Código Processing
```js
// Importamos librería para comunicación serial
import processing.serial.*;
// Importamos librería Minim para manejar audio
import ddf.minim.*;

// Declaramos el objeto serial para comunicarnos con Arduino
Serial myPort;
// Objeto principal de Minim
Minim minim;
// Array de reproductores de audio (3 pistas)
AudioPlayer[] players;
// Variable para guardar el índice de la pista que está sonando
int currentTrack = -1;  // -1 significa que no hay pista activa al inicio

void setup() {
  size(400, 200); // Ventana de 400x200 píxeles
  
  // --- Configuración del puerto serial ---
  printArray(Serial.list()); // Muestra en consola la lista de puertos disponibles
  myPort = new Serial(this, Serial.list()[0], 9600); // Abrimos el primer puerto de la lista a 9600 baudios
  
  // --- Configuración de audio ---
  minim = new Minim(this); // Inicializamos Minim
  players = new AudioPlayer[3]; // Creamos un array de 3 reproductores
  
  // Cargamos los 3 archivos de audio desde la carpeta "data"
  players[0] = minim.loadFile("sound-effect-happy-birthday-music-box-333245.mp3", 2048); 
  players[1] = minim.loadFile("sound-effect-christmas-santa-ho-ho-ho-255386.mp3", 2048); 
  players[2] = minim.loadFile("083195_small-group-of-kids-saying-quottrick-or-treatquot-46693.mp3", 2048); 
}

void draw() {
  background(0); // Fondo negro
  fill(255);     // Color blanco para el texto
  textSize(16);  // Tamaño del texto
  
  // Mostramos en pantalla qué botón está activo
  text("Botón actual: " + (currentTrack == -1 ? "ninguno" : currentTrack), 20, 40);
}

void serialEvent(Serial myPort) {
  // Leemos la cadena que llega desde Arduino hasta el salto de línea
  String inString = trim(myPort.readStringUntil('\n'));
  
  // Si no llega nada, salimos
  if (inString == null) return;

  // --- Si el mensaje recibido empieza con "B" significa que es un botón ---
  if (inString.startsWith("B")) {
    // Quitamos la letra "B" y separamos el mensaje en partes (ejemplo "0:0")
    String[] parts = split(inString.substring(1), ':');
    
    // Si realmente recibimos dos partes (índice y estado)
    if (parts.length == 2) {
      int buttonIndex = int(parts[0]); // Número del botón (0,1,2)
      int state = int(parts[1]);       // Estado del botón (0 = presionado, 1 = suelto)
      
      // Si el botón fue presionado (LOW = 0 en Arduino)
      if (state == 0) { 
        playTrack(buttonIndex); // Llamamos a la función para reproducir la pista correspondiente
      }
    }
  }
}

// --- Función que reproduce una pista según el botón ---
void playTrack(int index) {
  // Si ya había una pista sonando, la pausamos y la rebobinamos al inicio
  if (currentTrack != -1 && players[currentTrack].isPlaying()) {
    players[currentTrack].pause();
    players[currentTrack].rewind();
  }
  
  // Reproducimos en bucle la pista seleccionada
  players[index].loop();
  
  // Actualizamos la variable para saber cuál es la pista activa
  currentTrack = index;
}
```
##### Entrega 1: fusión ejercicio n°6 y n°7

Este ejercicio se basa en el ejercicio n° 6: Arduino, Processing con potenciómetro y el ejercicio n°7: Arduino, Processing con pulsador.
El ejercicio tiene tres objetivos clave, que es la creación de múltiples círculos, control dinámico de tamaño/color y efecto de desvanecimiento cuando de mueve de derecha a izquierda.
Obtuve ayuda de la inteligencia artificial géminis, donde le pregunte que me juntara estos dos ejercicios donde los círculos solo tienen que aparecer, mover y cambiar de color cuando se mueva el potenciómetro.


###### Código Arduino
```js
// CONEXIÓN: Potenciómetro Pin Central a A0. Extremos a 5V y GND.

int sensorPin = 0;     // Pin para el potenciómetro (A0)
int analogValue = 0;
int lastAnalogValue = 0;
// Si el valor cambia en más de 5 unidades, se considera movimiento.
const int threshold = 5;

void setup() {
  Serial.begin(9600);
}

void loop() {
  int rawValue = analogRead(sensorPin);
 
  // Escala el rango COMPLETO (0-1023) a 0-255.
  analogValue = map(rawValue, 0, 1023, 0, 255);
 
  // ------------------------------------
  // LÓGICA CLAVE: DETECCIÓN DE MOVIMIENTO
  // ------------------------------------
  // Solo si la diferencia es mayor que el umbral, enviamos el dato.
  if (abs(analogValue - lastAnalogValue) > threshold) {
   
    // 1. Envía el valor (¡Esto es el evento de movimiento!)
    Serial.println(analogValue);
   
    // 2. Actualiza la última lectura para la próxima comparación
    lastAnalogValue = analogValue;
  }
 
  delay(10); // Estabilidad en la transmisión
}
```
###### Código Processing
```js
import processing.serial.*;

Serial myPort;          // Objeto de la clase Serial
// Almacena las propiedades de los círculos (X, Y, y Diámetro Z)
ArrayList<PVector> circles; 

// Variable que guarda el último valor recibido para control de tamaño/color.
int sensorVal = 0;      

void setup() {
  background(0); 
  size(1080, 720);
  noStroke();
  
  circles = new ArrayList<PVector>();
  
  // ----------------------------------------------------
  // CONFIGURACIÓN SERIAL: AJUSTA ESTA LÍNEA A TU PUERTO
  // ----------------------------------------------------
  println("Puertos disponibles: " + Serial.list());
  // Elige el puerto correcto (ej: Serial.list()[0] o "COM3")
  myPort = new Serial(this, Serial.list()[0], 9600);
}

void draw() {
  // Limpia el fondo, pero con una pequeña transparencia para un efecto de "rastro"
  fill(0, 30); 
  rect(0, 0, width, height); 
  
  // 1. LECTURA SERIAL (El Bloque de Eventos)
  if (myPort.available() > 0) {
    String val = myPort.readStringUntil('\n'); 
    
    if (val != null) {
      try {
        int receivedVal = Integer.valueOf(val.trim());
        
        // **ACTUALIZACIÓN:** Guardar el valor para el color y tamaño
        sensorVal = receivedVal; 
        
        // -------------------------------------------
        // 2. EVENTO: CREAR Y ALMACENAR EL CÍRCULO
        // -------------------------------------------
        // La posición del nuevo círculo es aleatoria
        float newX = random(width);
        float newY = random(height);
        
        // Almacenamos X, Y, y el TAMAÑO actual (Z) en el PVector
        // Esto es CLAVE: El círculo "hereda" el tamaño que tenía el sensor en el momento de su creación.
        float creationDiameter = map(sensorVal, 0, 255, 40, 300);
        PVector newCircle = new PVector(newX, newY, creationDiameter);
        circles.add(newCircle);
      }
      catch(Exception e) {
        // Ignorar errores de parsing
      }
    }
  }  
  
  // 3. DIBUJAR Y REDIMENSIONAR TODOS LOS CÍRCULOS
  
  // Los parámetros dinámicos (Tamaño/Color) se basan en el último sensorVal recibido.
  
  // Control de Color Rojo: Mapea el valor (0-255) al componente Rojo
  float r = map(sensorVal, 0, 255, 100, 255);
  
  // Control de Color Verde: Mapea el valor (0-255) al componente Verde
  float g = map(sensorVal, 0, 255, 50, 200);
  
  // Dibujar todos los círculos
  for (PVector c : circles) {
    // Aplica el color dinámico (actual) a todos los círculos
    fill(r, g, 0); 
    
    // **NOTA CLAVE:** Usamos el componente Z (el tamaño inicial) para la posición.
    // Para que todos se muevan y redimensionen a la vez, debemos usar el sensorVal
    
    // Redimensionamiento y Movimiento Dinámico:
    float currentDiameter = map(sensorVal, 0, 255, 40, 300);
    
    // Movimiento Horizontal (opcional, pero incluido para hacer algo dinámico con la X)
    float currentX = map(sensorVal, 0, 255, 0, width);
    
    // Dibujar el círculo en su posición inicial (c.y) con la nueva X y diámetro
    ellipse(currentX, c.y, currentDiameter, currentDiameter);
  }
  
  // OPCIONAL: Limitar el número de círculos en pantalla
  if (circles.size() > 10) {
    circles.remove(0); 
  }
}
```
<img src="https://raw.githubusercontent.com/antito666/interfazII/refs/heads/main/img/Entrega_1.webp"/>
<img src="https://raw.githubusercontent.com/antito666/interfazII/refs/heads/main/img/entrega_1.png"/>

##### Ejercicio n°10: Sensor Sharp
###### Código Arduino
```js
// Definir el pin del sensor Sharp
int sharpPin = A0;

void setup() {
  Serial.begin(9600); // Iniciar comunicación serial
}

void loop() {
  int sensorValue = analogRead(sharpPin); // Leer valor del sensor
  Serial.println(sensorValue); // Enviar valor a Processing
  delay(100); // Esperar un momento
}
```
###### Código Processing
```js
import processing.serial.*;

Serial myPort;  // Create object from Serial class
static String val;    // Data received from the serial port
int sensorVal = 0;

void setup()
{
  background(0); 
  //fullScreen(P3D);
   size(1080, 720);
   noStroke();
  noFill();
  String portName = "COM5";// Change the number (in this case ) to match the corresponding port number connected to your Arduino. 

  myPort = new Serial(this, Serial.list()[0], 9600);
}

void draw()
{
  if ( myPort.available() > 0) {  // If data is available,
  val = myPort.readStringUntil('\n'); 
  try {
   sensorVal = Integer.valueOf(val.trim());
  }
  catch(Exception e) {
  ;
  }
  println(sensorVal); // read it and store it in vals!
  }  
 //background(0);
  // Scale the mouseX value from 0 to 640 to a range between 0 and 175
  float c = map(sensorVal, 0, width, 0, 400);
  // Scale the mouseX value from 0 to 640 to a range between 40 and 300
  float d = map(sensorVal, 0, width, 40,500);
  fill(255, c, 0);
  ellipse(width/2, height/2, d, d);   

}
```
<img src="https://raw.githubusercontent.com/antito666/interfazII/refs/heads/main/img/Sensor_Sharp.png"/>

##### Ejercicio n°11: Sensor de humedad (DFRobot)
###### Código Arduino
```js
/*******************************
           Conexión:
             VCC-5V
             GND-GND
             S-Analog pin A0

Puedes ponder el sensor en la palma de tu mano
para sensar la humedad de  tu palma.
 ********************************/

void setup()
{
  Serial.begin(9600);// abre el puerto serial y Establece la velocidad en baudios a 9600 bps
}
void loop()
{
  int sensorValue;
  sensorValue = analogRead(0);   //conectar el sensor de humedad al pin analogo 0
  Serial.println(sensorValue); //imprime el valor a serial.
  delay(200);
}
```
