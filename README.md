# Projeto_IrrigaArdu
- Sistema de Irrigação Automática


  Este projeto é um sistema de irrigação automática inteligente com base em IoT, utilizando o NodeMCU ESP8266, um sensor de umidade do solo e uma bomba de água acionada via relé. O sistema lê periodicamente a umidade do solo e, caso detecte níveis abaixo do limiar definido, aciona automaticamente a irrigação. A comunicação ocorre via protocolo MQTT, permitindo o monitoramento remoto e o acionamento manual por meio de tópicos MQTT. Contudo, devido problemas com o hardware da versão física, não foi possível conectar via protocolo MQTT, porém outra versão foi na plataforma WOKWI, no qual conseguimos avançar com todos os procedimentos requisitados para teste do projeto. O projeto IrrigaArdu é ideal para hortas, jardins ou vasos de plantas que requerem irrigação automatizada e controle à distância. 
O código realiza a automação da irrigação de uma planta ou solo com base na leitura de um sensor de umidade conectado a um ESP32. Ele lê periodicamente os valores analógicos do sensor no pino 34 e, caso o nível de umidade esteja abaixo de um valor mínimo predefinido, aciona uma bomba de água conectada ao pino 2. O sistema permite alternar entre os modos automático e manual por meio de comandos enviados via protocolo MQTT, utilizando um broker público (HiveMQ). No modo manual, é possível ligar ou desligar a bomba remotamente através de mensagens MQTT. O código também envia leituras periódicas de umidade e mensagens de alerta quando a umidade está crítica. Toda a lógica está implementada no arquivo principal do projeto. 

 
- Principais funções do código 

Conexão à rede Wi-Fi e ao broker MQTT; 
Leitura da umidade do solo via pino analógico; 
Publicação da umidade em um tópico MQTT (/irrigacao/umidade); 
Recebimento de comandos no tópico (/irrigacao/comando); 
Acionamento do relé para ligar/desligar a bomba de água; 
Lógica de irrigação automática baseada no nível de umidade. 
Envia os dados e o status da bomba para tópicos MQTT, permitindo monitoramento remoto. 

 
- Hardware Utilizado 

  NodeMCU ESP8266 
  Sensor de umidade do solo v2.0 
  Módulo Relé 5V  
  Mini-bomba d'água 12V 
  Fonte 12V DC, Protoboard 
  Jumpers 
  Protoboard breadboard 
  Resistores 
 
 

5.  Interfaces, Protocolos e Comunicação 

Interface Wi-Fi: Conexão com rede local usando o ESP8266. 
Protocolo de Comunicação: MQTT (TCP/IP). 

Tópicos utilizados:

/irrigacao/umidade – publicação do nível de umidade. 

/irrigacao/comando – subscrição para comandos de irrigação manual. 
 

 

 

 

 
