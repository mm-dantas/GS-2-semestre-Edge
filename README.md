# Monitoramento De Tensão Elétrica
## Sobre o Projeto
O Dispositivo de Monitoramento de Tensão Elétrica foi desenvolvido para auxiliar no combate às perdas no sistema elétrico, promovendo eficiência energética e sustentabilidade. Utilizando sensores, atuadores e conectividade MQTT, o dispositivo realiza monitoramento em tempo real da tensão elétrica e temperatura do próprio dispositivo, exibindo as informações em um display local e enviando dados para monitoramento remoto.

## Funcionalidades
O sistema realiza as seguintes funcionalidades principais:
- **Monitoramento de Tensão Elétrica:** Mede a tensão elétrica em tempo real e categoriza o estado da energia em: sem energia, energia ruim, energia ok e energia alta.
- **Medição de Temperatura:** Utiliza um sensor DHT22 para detectar a temperatura do dispositivo e prever falhas.
- **Controle Remoto via MQTT:**
   - Envia dados de tensão e temperatura para um broker MQTT.
   - Recebe comandos para operar um servo motor (trava do dispositivo).
- **Interface Local:**
  - LEDs indicativos de estado da energia.
  - Display LCD para exibição das leituras e mensagens.

## Tecnologias Utilizadas
**Hardware:**
  - ESP32 (Microcontrolador).
  - Sensor DHT22 (Temperatura).
  - Servo motor (Trava).
  - LEDs indicativos (Verde, Amarelo e Vermelho.
  - Display LCD I2C 16x2.
  - Potenciômetro (Simulação de variações na tensão).
 
**Software:**
  - Linguagem: C++.
  - Conexão: WiFi (MQTT para comunicação).
  - Wokwi (Simulador de eletrônica online).
  - Node-Red:
       - mqtt in.
       - mqtt out.
       - gauge.
       - chart.
       - inject.

**Bibliotecas necessárias:**
  - PubSubClient.
  - DHT Sensor Library.
  - ESP32Servo.
  - LiquidCrystal_I2C.

## Instruções de Uso
**Requisitos**
- Placa ESP32.
- IDE Arduino (versão mais recente).
- Conexão Wi-Fi disponível.
- Broker MQTT configurado (exemplo: HiveMQ).
- Bibliotecas necessárias:
  - PubSubClient
  - DHT Sensor Library
  - ESP32Servo
  - LiquidCrystal_I2C
**Configuração**
1. Clone o Repositório: `git clone https://github.com/mm-dantas/GS-2-semestre-Edge.git`
2. Configure a IDE Arduino:
   - Selecione a placa ESP32.
   - Instale as bibliotecas mencionadas nos requisitos.
3. Ajuste as Credenciais Wi-Fi:
   No código, substitua as variáveis ssid e password com os dados da sua rede Wi-Fi.
4. Configuração do Broker MQTT:
- Caso utilize outro broker, atualize as variáveis:
   - BROKER_MQTT
   - BROKER_PORT
**Executando o Projeto**
1. Faça o upload do código para o ESP32.
2. Conecte os componentes ao hardware conforme descrito no esquema.
3. Abra o monitor serial para verificar logs de execução.

## Simulação no Wokwi

Você pode testar este projeto diretamente no simulador Wokwi através do link abaixo:

[![Simular no Wokwi](https://img.shields.io/badge/Wokwi-Simular%20Projeto-blue?style=for-the-badge&logo=arduino)](https://wokwi.com/projects/414640233393353729)
