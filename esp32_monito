#include <WiFi.h>
#include <PubSubClient.h>
#include <LiquidCrystal_I2C.h>
#include <DHT.h>
#include <ESP32Servo.h>

const char *BROKER_MQTT = "broker.hivemq.com";
const int BROKER_PORT = 1883;
const char *ID_MQTT = "esp32_motor_control";
const char *TOPIC_ENERGIA = "fiap/iot/energia";
const char *TOPIC_TEMPERATURA = "fiap/iot/temperatura";
const char *TOPIC_COMANDO = "fiap/iot/comando";


LiquidCrystal_I2C lcd(0x27, 16, 2);

int ledVerde = 2;
int ledAmarelo = 4;
int ledVermelho = 5;

Servo trava;

#define DHTPIN 12           
#define DHTTYPE DHT22      

DHT dht(DHTPIN, DHTTYPE);  

const char* ssid = "Wokwi-GUEST";
const char* password = "";

WiFiClient espClient;
PubSubClient MQTT(espClient);


void callback(char* topic, byte* payload, unsigned int length) {
  Serial.print("Mensagem recebida no tópico: ");
  Serial.println(topic);

  // Converte a mensagem recebida para string
  String message = "";
  for (int i = 0; i < length; i++) {
    message += (char)payload[i];
  }

  Serial.print("Mensagem: ");
  Serial.println(message);

  // Verifica se a mensagem recebida é "1" ou "0"
  if (message == "1") {
    Serial.println("Comando '1' recebido! Abrindo o servo...");
    trava.write(180);  // Abre o servo (posição 180 graus)
  } 
  else if (message == "0") {
    Serial.println("Comando '0' recebido! Fechando o servo...");
    trava.write(0);  // Fecha o servo (posição 0 graus)
  }
}


void setup() {
  Serial.begin(115200);  // Inicializa a comunicação serial
  analogReadResolution(12);

  pinMode(ledVerde, OUTPUT);
  pinMode(ledAmarelo, OUTPUT);
  pinMode(ledVermelho, OUTPUT);
  trava.attach(13);

  lcd.init();
  lcd.backlight();

  WiFi.begin(ssid, password);
  Serial.print("Conectando ao Wi-Fi");

  while (WiFi.status() != WL_CONNECTED) {
    delay(1000);
    Serial.print(".");
  }

  Serial.println("");
  Serial.println("WiFi conectado!");
  Serial.print("IP obtido: ");
  Serial.println(WiFi.localIP());

  MQTT.setServer(BROKER_MQTT, BROKER_PORT);

  MQTT.setCallback(callback);
}

void reconnectMQTT() {
  while (!MQTT.connected()) {
    Serial.print("Tentando conectar ao Broker MQTT...");
    if (MQTT.connect(ID_MQTT)) {
      Serial.println("Conectado ao broker MQTT!");
      MQTT.subscribe(TOPIC_COMANDO);
    } else {
      Serial.print("Falha na conexão, tentando novamente em 2 segundos.");
      delay(2000);
    }
  }


}

void loop() {
  if (!MQTT.connected()) {
    reconnectMQTT();
  }
  MQTT.loop();

  int valorAnalogico = analogRead(34);  // Lê o valor no pino GPIO34
  float tensaoVindaDoEsp = valorAnalogico * (3.3 / 4095.0);  // Converte o valor para tensão (3.3V para o ESP32)
  float simulandoTensaoCasa = tensaoVindaDoEsp * 66.7;

  float h = dht.readHumidity();  // Lê a umidade
  float t = dht.readTemperature();  // Lê a temperatura
  String payload2 = String(t);
  String payload = String(simulandoTensaoCasa);

  MQTT.publish(TOPIC_ENERGIA, payload.c_str());
  MQTT.publish(TOPIC_TEMPERATURA, payload2.c_str());

  // Exibe a tensão na primeira linha
  lcd.setCursor(0, 0);
  lcd.print("Tensao: ");
  lcd.print(simulandoTensaoCasa);
  lcd.print("V");

  delay(2000);  // Espera 2 segundos
  lcd.clear();  // Limpa a tela LCD

  // Verifica o estado da energia
  if (simulandoTensaoCasa < 10) {
    lcd.setCursor(0, 0);
    lcd.print("Sem Energia");
    digitalWrite(ledVerde, LOW);
    digitalWrite(ledAmarelo, LOW);
    digitalWrite(ledVermelho, HIGH);
  } else if (simulandoTensaoCasa >= 10 && simulandoTensaoCasa < 150) {
    lcd.setCursor(0, 0);
    lcd.print("Energia Ruim");
    digitalWrite(ledVerde, LOW);
    digitalWrite(ledAmarelo, HIGH);
    digitalWrite(ledVermelho, LOW);
  } else if (simulandoTensaoCasa >= 150 && simulandoTensaoCasa <= 240) {
    lcd.setCursor(0, 0);
    lcd.print("Energia OK");
    digitalWrite(ledVerde, HIGH);
    digitalWrite(ledAmarelo, LOW);
    digitalWrite(ledVermelho, LOW);
  } else {
    lcd.setCursor(0, 0);
    lcd.print("Energia Alta");
    digitalWrite(ledVerde, LOW);
    digitalWrite(ledAmarelo, LOW);
    digitalWrite(ledVermelho, HIGH);
  }

  delay(2000);  // Espera 2 segundos
  lcd.clear();

  // Exibe a temperatura e umidade
  lcd.setCursor(0, 0);
  lcd.print("Temp disp: ");
  lcd.setCursor(0, 1);
  lcd.print(t);
  lcd.print(" C");

  MQTT.publish(TOPIC_ENERGIA, payload.c_str());
  MQTT.publish(TOPIC_TEMPERATURA, payload2.c_str());

  delay(1500);  // Espera 1,5 segundo
  lcd.clear();

}
