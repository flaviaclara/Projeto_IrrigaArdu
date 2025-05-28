# Projeto_IrrigaArdu
Sistema de Irrigação Automática

1. Descrição 

Este projeto é um sistema de irrigação automática inteligente com base em IoT, utilizando o NodeMCU ESP8266, um sensor de umidade do solo e uma bomba de água acionada via relé. O sistema lê periodicamente a umidade do solo e, caso detecte níveis abaixo do limiar definido, aciona automaticamente a irrigação. A comunicação ocorre via protocolo MQTT, permitindo o monitoramento remoto e o acionamento manual por meio de tópicos MQTT. O projeto é ideal para hortas, jardins ou vasos de plantas que requerem irrigação automatizada e controle à distância. 

2. Software 

O código realiza a automação da irrigação de uma planta ou solo com base na leitura de um sensor de umidade do solo. Ele usa um NodeMCU (ESP8266) para ler os valores analógicos do sensor conectado ao pino A0. Se o valor lido ultrapassar um determinado limiar (indicando que o solo está seco), o sistema liga uma bomba de água conectada ao pino D1 através de um relé. Caso o solo esteja úmido, a bomba permanece desligada. O código desenvolvido está disponível neste repositório no arquivo main.ino.  

2.1 Trechos comentados / pseudocódigo 

1. Inclusão das bibliotecas 

#include <ESP8266WiFi.h>      // Conexão com Wi-Fi 
#include <PubSubClient.h>     // Cliente MQTT para envio de dados 
2. Credenciais da rede Wi-Fi 

const char* ssid = "ArrigArdu"; 
const char* password = ""; 

3. Configuração do servidor MQTT 

const char* mqtt_server = "192.168.1.100"; // IP do broker local (ex: Mosquitto) 
const int mqtt_port = 1883; 

4. Definição dos pinos e limiares 

#define SENSOR_UMIDADE A0 
#define RELE_BOMBA     D1 
#define LIMIAR_SECO    700     // O valor 700 é um limite empírico; acima dele, considera-se que o solo está seco. 

5. Função de conexão Wi-Fi 
void setup_wifi() { 
  WiFi.begin(ssid, password); 
  while (WiFi.status() != WL_CONNECTED) { 
    delay(500); 
    Serial.print("."); 
  } 
} 

6. Função de conexão ao MQTT 

void reconnect_mqtt() { 
  while (!client.connected()) { 
    client.connect("ESP8266Client", mqtt_user, mqtt_pass); 
    delay(5000); 
  } 
} 

7. Setup inicial 

void setup() { 
  Serial.begin(115200); 
  pinMode(RELE_BOMBA, OUTPUT); 
  digitalWrite(RELE_BOMBA, LOW); 
  setup_wifi(); 
  client.setServer(mqtt_server, mqtt_port);} 

8. Loop principal 

void loop() { 
  if (!client.connected()) { 
    reconnect_mqtt(); 
  } 
  client.loop(); 

9. Leitura da umidade e controle da bomba 
int valor = analogRead(SENSOR_UMIDADE); 
 
if (valor > LIMIAR_SECO) { 
  digitalWrite(RELE_BOMBA, HIGH); 
  client.publish("IrrigArdu/solo/status", "BOMBA_LIGADA"); 
} else { 
  digitalWrite(RELE_BOMBA, LOW); 
  client.publish("IrrigArdu/solo/status", "BOMBA_DESLIGADA"); 
} 
 
3. Principais funções do código 

Conexão à rede Wi-Fi e ao broker MQTT; 
Leitura da umidade do solo via pino analógico; 
Publicação da umidade em um tópico MQTT (/irrigacao/umidade); 
Recebimento de comandos no tópico (/irrigacao/comando); 
Acionamento do relé para ligar/desligar a bomba de água; 
Lógica de irrigação automática baseada no nível de umidade. 
Envia os dados e o status da bomba para tópicos MQTT, permitindo monitoramento remoto. 

3.1 Explicação das bibliotecas usadas 

Biblioteca: ESP8266WiFi.h 

Biblioteca nativa do ESP8266. 
Permite conectar o módulo a redes Wi-Fi. 
Comandos: WiFi.begin(), WiFi.status(), WiFi.localIP(), etc.

Biblioteca: PubSubClient.h
Biblioteca de cliente MQTT leve e eficiente. 
Suporta conexão a brokers MQTT  
Funções principais: 
setServer() – define IP e porta do broker 
connect() – conecta ao broker 
publish() – publica mensagens em tópicos 
subscribe() – assina tópicos para receber mensagens (não usada neste projeto) 
loop() – mantém a conexão e trata mensagens recebidas 

Biblioteca: WiFiClient 
Objeto auxiliar necessário para o PubSubClient funcionar com redes Wi-Fi. 
 

4.Hardware Utilizado 

- NodeMCU ESP8266 
- Sensor de umidade do solo v2.0 
- Módulo Relé 5V  
- Mini-bomba d'água 12V 
- Fonte 12V DC, Protoboard 
- Jumpers 
- Protoboard breadboard 
- resistores 
 
 

5.  Interfaces, Protocolos e Comunicação 

Interface Wi-Fi: Conexão com rede local usando o ESP8266. 
Protocolo de Comunicação: MQTT (TCP/IP). 
Tópicos utilizados: 
/irrigacao/umidade – publicação do nível de umidade. 
/irrigacao/comando – subscrição para comandos de irrigação manual. 
 

 

 

 

 
