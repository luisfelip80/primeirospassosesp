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
