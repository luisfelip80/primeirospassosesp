# Primeiro baixar o Arduino IDE
https://www.arduino.cc/en/Main/Software

# Esp32 on arduino IDE
Abra sua Arduino IDE, clique em Arquivos->Preferencias, ou aperte em seu teclado Ctrl + Virgula.
Na tela que se abriu vá até: URLs: Adicionais para Gerenciadores de Placas e clique no ícone ao lado conforme a imagem abaixo.
Cole o seguinte link: https://dl.espressif.com/dl/package_esp32_index.json na tela que se abriu e click em OK.
Depois de carregar os arquivos da ESP32 temos que fazer a instalação. Vá em Ferramentas->Placa->Gerenciador de Placas.
Espere alguns segundos até que o campo de texto esteja habilitado e digite ESP32. O arquivo de instalação da placa aparecerá, clique em INSTALAR, quando a instalação terminar clique em FECHAR.
Agora abra Ferramentas->Placa e veja se as placas ESP32 estão listadas na Arduino IDE assim como na imagem abaixo.

# Esp32 Pinout
![ESP32-pinout-mapping](https://user-images.githubusercontent.com/16996822/69886934-42fcae00-12c3-11ea-86c5-c46d5792c2ab.png)

# Esp32 Helltec LoRa on arduino IDE
Abra sua Arduino IDE, clique em Arquivos->Preferencias, ou aperte em seu teclado Ctrl + Virgula.
Na tela que se abriu vá até: URLs: Adicionais para Gerenciadores de Placas e clique no ícone ao lado conforme a imagem abaixo.
Cole o seguinte link: https://docs.heltec.cn/download/package_heltec_esp32_index.json na tela que se abriu e click em OK.
Depois de carregar os arquivos da Heltec ESP32 temos que fazer a instalação. Vá em Ferramentas->Placa->Gerenciador de Placas.
Espere alguns segundos até que o campo de texto esteja habilitado e digite Heltec ESP32. O arquivo de instalação da placa aparecerá, clique em INSTALAR, quando a instalação terminar clique em FECHAR.
Agora abra Ferramentas->Placa e veja se as placas ESP32 estão listadas na Arduino IDE assim como na imagem abaixo.

# Esp32 Heltec Pinout
![pinout](https://user-images.githubusercontent.com/16996822/69886820-9fab9900-12c2-11ea-9ead-1f64835b67a3.jpg)


<h1> Introdução a estruturas de programação em C/C++</h>
<h2>"#include<...>" - Adiciona alguma biblioteca de programação já pronta para seu código, você pode agora usar as funções feitas por alguém em seu código.</h2>
<h2>"void setup(){ ... }" O "void" segnifica que essa será uma função sem retorno algum, ela só executa e não retorna nada aquele que a chamou, "setup()" é uma função base usada no arduino que é executada apenas uma vez tudo aquilo que está dentro de suas chaves "{}", nela são normalmente feitas as configurações de portas, iniciação de bibliotecas, objetocs, coisas feitas apenas uma vez e no inicio do código.</h2>
<h2>"void loop(){...}", assim como a anterior ela não retorna nada, diferente de uma outra função como "int contador(){}" que tem como rentorno uma variável "int" - "inteira" para aquele que chamou essa função. No loop é executado milhares de vezes e sem para, normalmente usado para fazer verificações constantes.</h>
<h2>"pinMode('Qual pino você quer' , 'ele será entrada ou saída)" - Usada parta configurar os GPIOs como entrada - "INPUT" ou saída - "OUTPUT". Ex: pinMode(14,INPUT) - defini o pino 14 como entrada.</h2>
<h2>digitalWrite( ' pino ', ' o que escrever (HIGH ou LOW)') - digitalRead( 'de qual pino ler')  - basicamente fazer leituras dos pinos e escrita</h2>
<h2>Serial.begin(9600) - Serial.println("bla") - o primeiro inicia o monitor serial do arduido com a velocidade de 9600 e o outro imprime na tela algo</h2>

<h1> Primeiro experimento com esp32</h1>

<h2>Controlar um LED usando esp32</h2>

![LED-blinking-ESP32](https://user-images.githubusercontent.com/16996822/69887049-bf8f8c80-12c3-11ea-8f6b-f402e40ce9cc.jpg)

<h2>Código</h2>

```
void setup(){
  pinMode(22, OUTPUT);
}

void loop() {
  digitalWrite(22, HIGH); 
  delay(1000); 
  digitalWrite(22 LOW); 
  delay(1000); 
}

```

<h1>Segundo experimento</h1>

<h2>Controlar um LED usando um botão</h2>

![Push-button-interfacing-with-ESP32](https://user-images.githubusercontent.com/16996822/69887283-f4501380-12c4-11ea-8ac5-c4c7ed5f4d6b.jpg)

<h2>Código</h2>

```
const int LEDPIN = 22; 
void setup(){
  pinMode(LEDPIN, OUTPUT);
  pinMode(PushButton, INPUT);
}

void loop(){
  int Push_button_state = digitalRead(PushButton);
  if ( Push_button_state == HIGH ){ 
    digitalWrite(LEDPIN, HIGH); 
  }
  else {
    digitalWrite(LEDPIN, LOW); 
  }
}

```

<h1>Terceiro experimento</h1>

<h2>Variar valores com um potenciometro</h2>

![measure-analog-voltage-ESP32](https://user-images.githubusercontent.com/16996822/69887400-793b2d00-12c5-11ea-81d1-758d1508fca6.jpg)


<h2>Código</h2>

```
const int Analog_channel_pin= 15;
int ADC_VALUE = 0;
int voltage_value = 0; 
void setup() {
  Serial.begin(115200);
}
void loop() {
  ADC_VALUE = analogRead(Analog_channel_pin);
  Serial.print("ADC VALUE = ");
  Serial.println(ADC_VALUE);
  delay(1000);
  voltage_value = (ADC_VALUE * 3.3 ) / (4095);
  Serial.print("Voltage = ");
  Serial.print(voltage_value);
  Serial.print("volts");
  delay(1000);
}

```

<h1>Quarto experimento</h1>

<h2>Ler sensor de temperatura e umidade</h2>

![Wiring-diagram-of-DHT22-interfacing-with-ESP32](https://user-images.githubusercontent.com/16996822/69887555-46456900-12c6-11ea-9164-df13dce8bb51.jpg)

<h2> Primeiro vá em <b>Sketch</b> >> <b>Include Library</b> e click no <b>Manage Libraries</b>.
<h2> Então pesquise por DHT22 e instale a biblioteca <b>DHT sensor library</b> by <b>Adafruit</b>.

<h2>Código Simples de leitura</h2>

```
#include <Wire.h>
#include "DHT.h"

#define DHTTYPE DHT22 // DHT 22 (AM2302), AM2321
//DHT Sensor;
uint8_t DHTPin = 4; 
DHT dht(DHTPin, DHTTYPE); 
float Temperature;
float Humidity;

String header;

void setup() {
	Serial.begin(9600);
	pinMode(DHTPin, INPUT);
	dht.begin();
}

void loop(){

	Temperature = dht.readTemperature(); 
	Humidity = dht.readHumidity(); 
	Serial.println("Temperatura" + String(Temperature) +"Umidade" + String(Humidity));
}

```

<h2>Código de leitura com Web server</h2>

```
#include <WiFi.h> 
#include <Wire.h>
#include "DHT.h"

// Uncomment one of the lines below for whatever DHT sensor type you're using!
//#define DHTTYPE DHT11 // DHT 11
//#define DHTTYPE DHT21 // DHT 21 (AM2301)
#define DHTTYPE DHT22 // DHT 22 (AM2302), AM2321
//DHT Sensor;
uint8_t DHTPin = 4; 
DHT dht(DHTPin, DHTTYPE); 
float Temperature;
float Humidity;

const char* ssid = "Pepino"; 
const char* password = "pepinoDenovo";

WiFiServer server(80);

String header;

void setup() {
	Serial.begin(115200);
	pinMode(DHTPin, INPUT);
	dht.begin();

	Serial.print("Connecting to Wifi Network");
	Serial.println(ssid);
	WiFi.begin(ssid, password);
	while (WiFi.status() != WL_CONNECTED) {
		delay(500);
		Serial.print(".");
	}
	Serial.println("");
	Serial.println("Successfully connected to WiFi.");
	Serial.println("IP address of ESP32 is : ");
	Serial.println(WiFi.localIP());
	server.begin();
	Serial.println("Server started");

}

void loop(){
	Temperature = dht.readTemperature(); 
	Humidity = dht.readHumidity(); 
	WiFiClient client = server.available();

	if (client) { 
		client.println("<!DOCTYPE html><html>");
		client.println("<head><meta name=\"viewport\" content=\"width=device-width, initial-scale=1\">");
		client.println("<link rel=\"icon\" href=\"data:,\">");
		client.println("</style></head><body><h1>ESP32 Web Server Reading sensor values</h1>");
		client.println("<h2>DHT11/DHT22</h2>");
		client.println("<h2>Microcontrollerslab.com</h2>");
		client.println("<table><tr><th>MEASUREMENT</th><th>VALUE</th></tr>");
		client.println("<tr><td>Temp. Celsius</td><td><span class=\"sensor\">");
		client.println(dht.readTemperature());
		client.println(" *C</span></td></tr>"); 
		client.println("<tr><td>Temp. Fahrenheit</td><td><span class=\"sensor\">");
		client.println(1.8 * dht.readTemperature() + 32);
		client.println(" *F</span></td></tr>"); 
		client.println("<tr><td>Humidity</td><td><span class=\"sensor\">");
		client.println(dht.readHumidity());
		client.println(" %</span></td></tr>"); 
		client.println("</body></html>");
	}
}

```
<h1>Quinto experimento</h1>

<h2>Ler sensor Ultrassonico</h2>

![esp-sr04](https://user-images.githubusercontent.com/16996822/69888189-5ad73080-12c9-11ea-812c-394b21a03176.png)


<h2>Código Simples de leitura</h2>

```
#define TRIGGER_PIN  5
#define ECHO_PIN     4

void setup() {
  Serial.begin (9600);
  pinMode(TRIGGER_PIN, OUTPUT);
  pinMode(ECHO_PIN, INPUT);
  pinMode(BUILTIN_LED, OUTPUT);
}

void loop() {
  long duration, distance;
  digitalWrite(TRIGGER_PIN, LOW);  // Added this line
  delayMicroseconds(2); // Added this line
  digitalWrite(TRIGGER_PIN, HIGH);
  delayMicroseconds(10); // Added this line
  digitalWrite(TRIGGER_PIN, LOW);
  duration = pulseIn(ECHO_PIN, HIGH);
  distance = (duration/2) / 29.1;
  Serial.print(distance);
  Serial.println(" cm");
  delay(1000);
}

```

<h1>Sexto experimento</h1>

<h2>Usar display LCD</h2>
<h2> Baixe a biblioteca do I2C LCD https://github.com/johnrickman/LiquidCrystal_I2C.</h>
<h>Você vai receber uma pasta comprimida .zip, descomprima.</h>
<h>Depois de descomprimir, você vai na pasta com nome LiquidCrystal_I2C-master e renomeia para LiquidCrystal_I2C.</h>
<h>Feche a IDE do arduino.</h>
<h>Copie a biblioteca na pasta de bibliotecas do arduino em algum lugar de seu computador.</h>

![I2C-LCD-interfacing-with-ESP3-wiring-diagram](https://user-images.githubusercontent.com/16996822/69888290-c7522f80-12c9-11ea-9253-fe94baf6a8ef.jpg)

<table>
	<thead>
		<th>ESP32 Development Board</th>
		<th>I2C LCD</th>
	</thead>
	<tbody>
		<tr>
			<td>GPIO22</td>
			<td>SCL</td>
		</tr>
		<tr>
			<td>GPIO21</td>
			<td>SDA</td>
		</tr>
		<tr>
			<td>GROUND</td>
			<td>GND</td>
		</tr>
		<tr>
			<td>3.3 Volts - VCC</td>
			<td>Vin</td>
		</tr>
</table>
<h2>Primeiro use esse código para encontrar o endereço do display</h2>

```
#include <Wire.h> // This library includes I2C communication functions 

void setup() {
Wire.begin();
Serial.begin(115200);
Serial.println("ESP32 scanning for I2C devices");
}

void loop() {
byte error_i2c, address_i2c;
int I2C_Devices;
Serial.println("Scanning started");
I2C_Devices = 0;
for(address_i2c = 1; address_i2c < 127; address_i2c++ )
{
Wire.beginTransmission(address_i2c);
error_i2c = Wire.endTransmission();
if (error_i2c == 0) {
Serial.print("I2C device found at address_i2c 0x");
if (address_i2c<16) 
{
Serial.print("0");
}
Serial.println(address_i2c,HEX);
I2C_Devices++;
}
else if (error_i2c==4) 
{
Serial.print("Unknow error_i2c at address_i2c 0x");
if (address_i2c<16) 
{
Serial.print("0");
}
Serial.println(address_i2c,HEX);
} 
}
if (I2C_Devices == 0) 
{
Serial.println("No I2C device connected \n");
}
else {
Serial.println("done I2C device searching\n");
}
delay(2000); 
}
```
![I2C-LCD-address-with-ESP32](https://user-images.githubusercontent.com/16996822/69888323-041e2680-12ca-11ea-9629-8c57ec05a947.jpg)

<h2>Use o código nesse código</h2>

```
#include <LiquidCrystal_I2C.h>

int totalColumns = 16;
int totalRows = 2;

LiquidCrystal_I2C lcd(0x27, totalColumns, totalRows);

void setup(){
lcd.init(); 
lcd.backlight(); // use to turn on and turn off LCD back light
}

void loop()
{
lcd.setCursor(0, 0);
lcd.print("Microcontrollerslab");
lcd.setCursor(0,1);
lcd.print("I2C LCD tutorial");
delay(1000);
lcd.clear(); 
lcd.setCursor(0, 0);
lcd.print("Static text");
delay(1000);
lcd.setCursor(0,1);
lcd.print("I2C LCD tutorial");
delay(1000);
lcd.clear(); 
}
```

<h1>Sétimo experimento</h1>

<h2>Servo motor</h2>

![ESP32-servo-motor-control-with-web-server-in-Arduino-IDE](https://user-images.githubusercontent.com/16996822/69888924-38dfad00-12cd-11ea-9909-aadcbddbb2c8.jpg)

Baixe a biblioteca do servo motor para Arduino IDE https://github.com/RoboticsBrno/ESP32-Arduino-Servo-Library.
Depois de baixar descomprima o .zip e mude o none de "ESP32-Arduino-Servo-Library-Master" para "ESP32-Arduino-Servo-Library"
Copie e cole na pasta de bibliotecas do Arduino em algum lugar de seu computador.

<h2>Código Simples</h2>

```
#include <Servo.h>
static const int servoPin = 13;  // defines pin number for PWM
Servo servo1;  // Create object for servo motor

void setup() {
	Serial.begin(115200);
	servo1.attach(servoPin);  // start the library 
}

void loop() {
	for(int posDegrees = 0; posDegrees <= 180; posDegrees++) {
		servo1.write(posDegrees);
		Serial.println(posDegrees);
		delay(20);
	}
	for(int posDegrees = 180; posDegrees >= 0; posDegrees--) {
		servo1.write(posDegrees);
		Serial.println(posDegrees);
		delay(20);
	}
}

```
<h2>Código Para controle usando um Potenciômetro</h2>

```
#include <Servo.h>

static const int servoPin = 13;
static const int potentiometerPin = 32;

Servo servo1;

void setup() {
	Serial.begin(115200);
	servo1.attach(servoPin);
}

void loop() {
	int servoPosition = map(analogRead(potentiometerPin), 0, 4096, 0, 180);
	servo1.write(servoPosition);
	Serial.println(servoPosition);
	delay(20);
}

```
