<h1 align="center">Checkpoint 6 - Edge Computing (IoT)</h1>

Este projeto tem como objetivo aplicar conceitos de **Internet das Coisas (IoT)** utilizando o **ESP32** como microcontrolador principal.  
A proposta é criar um sistema capaz de **monitorar variáveis ambientais** — como **temperatura, umidade e luminosidade** — e **enviar essas informações para um broker MQTT**, onde podem ser visualizadas ou tratadas por uma aplicação inteligente, como o **FIWARE**.  

Além do monitoramento, o projeto implementa um **atuador remoto (LED)** controlado via MQTT, simulando a automação de dispositivos em um ambiente real, como a **Vinheria Agnello**, que precisa manter condições ideais de conservação para seus vinhos.

---

## ⚙️ Funcionalidades Principais

- 🔌 **Conexão automática à rede Wi-Fi** configurada no código.  
- 📡 **Integração com broker MQTT** para envio e recebimento de dados.  
- 🌡️ **Leitura de temperatura e umidade** através do sensor **DHT11**.  
- 💡 **Leitura da luminosidade ambiente** utilizando um **sensor LDR**.  
- 💬 **Publicação periódica** dos dados no tópico MQTT `/TEF/device007/attrs`.  
- 💡 **Controle remoto do LED** via mensagens recebidas do broker (`on|off`).  
- 🔁 **Reconexão automática** em caso de falha de rede ou do servidor MQTT.  
- 🕒 Intervalo de leitura dos sensores configurado para **10 segundos**.

---

## 🧠 Visão Geral do Funcionamento

O sistema segue o fluxo ilustrado abaixo:

1. **Inicialização do ESP32** → Configura Wi-Fi, sensores e MQTT.  
2. **Coleta de dados** → O DHT11 mede temperatura e umidade; o LDR mede luminosidade.  
3. **Envio dos dados via MQTT** → As medições são publicadas no tópico `/TEF/device007/attrs`.  
4. **Recebimento de comandos** → O ESP32 escuta o tópico `/TEF/device007/cmd`, que recebe mensagens como `on` ou `off`.  
5. **Ação no atuador (LED)** → O LED é ligado ou desligado conforme o comando recebido.  
6. **Monitoramento em tempo real** → Os dados podem ser integrados com o **FIWARE**, **Node-RED**, **Grafana** ou outras plataformas IoT para visualização e automação.  

---

## 🔧 Etapas de Desenvolvimento

1. **Planejamento do projeto:** definição do cenário (Vinheria Agnello), escolha dos sensores e protocolo de comunicação (MQTT).  
2. **Montagem do circuito:** conexão do DHT11, LDR e LED nos pinos do ESP32 conforme o esquema.  
3. **Desenvolvimento do código:** programação em C++ (Arduino IDE) para conectar à rede, ler sensores, publicar e assinar tópicos MQTT.  
4. **Testes locais:** uso do **MQTT Explorer** ou **HiveMQ** para verificar o envio e recebimento das mensagens.  
5. **Integração com o FIWARE (opcional):** configuração do Orion Context Broker para armazenar e visualizar dados.  
6. **Ajustes e otimizações:** calibração dos sensores, intervalos de leitura e tratamento de reconexões.  

---

## 🧩 Estrutura do Código

O programa foi desenvolvido em **C++ para a plataforma Arduino (ESP32)** e é dividido em funções principais:

- **`setup()`** – Inicializa sensores, conexão Wi-Fi, MQTT e define o estado inicial do LED.  
- **`loop()`** – Mantém as conexões ativas e realiza a publicação periódica dos dados.  
- **`mqtt_callback()`** – Recebe e interpreta mensagens MQTT, controlando o LED conforme o comando recebido.  
- **`publishSensorData()`** – Lê os sensores DHT11 e LDR, monta a string de dados e publica no broker.  
- **`reconnectWiFi()` / `reconnectMQTT()`** – Funções responsáveis por garantir reconexão automática em caso de perda de sinal.  
- **`InitOutput()`** – Inicializa o LED e realiza um piscar rápido para indicar o boot do sistema.  

---

## 🏛️ Tecnologias e Bibliotecas Utilizadas

- **ESP32 (Wi-Fi integrado)**
- **DHT11** – Sensor de temperatura e umidade.  
- **LDR (Light Dependent Resistor)** – Sensor de luminosidade.  
- **Bibliotecas:**
  - `WiFi.h`
  - `PubSubClient.h`
  - `DHT.h`
  - `Adafruit_Sensor.h`

---

## 🔌 Conexões de Hardware

| Componente | Pino ESP32 | Descrição |
|-------------|------------|------------|
| DHT11       | 4          | Leitura de temperatura e umidade |
| LDR         | 34         | Leitura da luminosidade |
| LED (saída) | 2 (D4)     | Indicador de estado / Atuador |

---

## 💬 Comunicação MQTT

| Tipo | Tópico | Descrição |
|------|--------|------------|
| **Publicação** | `/TEF/device007/attrs` | Envia os dados de sensores (luminosidade, temperatura e umidade). |
| **Inscrição** | `/TEF/device007/cmd` | Recebe comandos para ligar/desligar o LED. |

**Formato das mensagens:**
- Publicação de sensores → `l|[luminosidade]|t|[temperatura]|h|[umidade]`

---

## 📊 Dashboard

O Dashboard **Node-RED** é uma ferramenta eficaz e visual para monitoramento em tempo real. Neste projeto, configuramos um Dashboard interativo para exibir os dados de **temperatura, umidade e luminosidade,** que são enviados periodicamente do ESP32 via MQTT.

### Como Funciona:

- **Leitura dos Sensores** – O ESP32 coleta dados do sensor DHT11 (temperatura e umidade) e do sensor LDR (luminosidade). Esses valores são então enviados para o broker MQTT.

- **Recebimento no Node-RED** – No Node-RED, criamos fluxos que escutam os tópicos MQTT, coletando as informações publicadas pelo ESP32.

- **Exibição dos Dados** – O Dashboard apresenta esses valores de forma visual e intuitiva, com gráficos e indicadores que mostram a temperatura, umidade e luminosidade em tempo real.


### Imagem do Dashboard

<img width="1585" height="576" alt="image" src="https://github.com/user-attachments/assets/2b90f69a-bfa5-4919-a17c-eba668710be3" />


---

## 👨‍💻 Autores

- **Guilherme Moura Gama** – RM: 562162  
- **Guilherme Ruiz Costa** – RM: 563236  
- **João Batista Lima Neto** – RM: 563426  
- **Júlio César Augusto Vieira** – RM: 563366  

---

## 🏫 Instituição

**FIAP**  
Curso: **Engenharia de Software**  
Disciplina: **Edge Computing And Computer Systems**  
Professor: **Lucas Demetrius Augusto**  

---

