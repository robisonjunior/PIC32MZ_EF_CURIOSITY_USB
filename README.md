\# Curiosity 2.0 PIC32 MZ EF - Pilha USB Microchip Serial COM Virtual



# USB CDC Device com MPLAB Harmony v3 (Controle de LEDs e Botões)

Arquivo de código-fonte (`app.c`) desenvolvido utilizando o framework **MPLAB Harmony v3**, implementando uma aplicação baseada em máquina de estados para comunicação **USB CDC (Virtual COM Port)**, controle de LEDs RGB e gerenciamento de múltiplos botões com envio de mensagens de status via USB.

---

## 📋 Funcionalidades

- **Classe USB CDC (Virtual COM Port):** Permite a comunicação bidirecional entre o microcontrolador e um PC host através de comandos seriais.
- **Controle de LEDs via Comandos Seriais:** O host pode enviar caracteres para ligar ou desligar os canais do LED RGB:
  - `B` / `b`: Liga / Desliga o LED Azul
  - `G` / `g`: Liga / Desliga o LED Verde
  - `R` / `r`: Liga / Desliga o LED Vermelho
- **Leitura de Múltiplos Botões (SW1 a SW4):**
  - **SW1:** Monitorado com rotina de *debounce* baseada em interrupções de início de quadro USB (SOF).
  - **SW2, SW3 e SW4:** Detectam bordas de descida para alternar estados de LEDs locais e reportar o evento via USB.
- **Máquina de Estados Robusta:** Gerenciamento de conexão USB, inicialização, leitura e escrita assíncrona.

---

## 🛠️ Comandos de Controle USB (CDC)

| Caracter Enviado pelo Host | Ação no Firmware | Mensagem Retornada |
| :---: | :--- | :--- |
| **`B`** | Liga o LED Azul (`RGB_LED_B_On`) | `\r\nLed RGB Azul Acesso...` |
| **`b`** | Desliga o LED Azul (`RGB_LED_B_Off`) | `\r\nLed RGB Azul Apagado...` |
| **`G`** | Liga o LED Verde (`RGB_LED_G_On`) | `\r\nLed RGB Verde Acesso...` |
| **`g`** | Desliga o LED Verde (`RGB_LED_G_Off`) | `\r\nLed RGB Verde Apagado...` |
| **`R`** | Liga o LED Vermelho (`RGB_LED_R_On`) | `\r\nLed RGB Vermelho Acesso...` |
| **`r`** | Desliga o LED Vermelho (`RGB_LED_R_Off`) | `\r\nLed RGB Vermelho Apagado...` |

---

## ⚙️ Arquitetura da Máquina de Estados

O arquivo `app.c` opera ciclicamente através de estados bem definidos gerenciados em `APP_Tasks()`:

1. **`APP_STATE_INIT`**: Abre a camada de dispositivo USB e registra os *callbacks* de eventos.
2. **`APP_STATE_WAIT_FOR_CONFIGURATION`**: Aguarda o host enumerar e configurar o dispositivo USB.
3. **`APP_STATE_SCHEDULE_READ`**: Agenda operações de leitura assíncrona para receber dados da porta virtual COM.
4. **`APP_STATE_WAIT_FOR_READ_COMPLETE / APP_STATE_CHECK_SWITCH_PRESSED`**: Monitora o buffer de recepção e o acionamento físico dos botões (SW1 a SW4).
5. **`APP_STATE_SCHEDULE_WRITE`**: Processa os comandos recebidos (caracteres seriais ou interrupções de botões) e monta o pacote de resposta.
6. **`APP_STATE_WAIT_FOR_WRITE_COMPLETE`**: Aguarda a conclusão do envio dos dados via USB antes de retornar ao ciclo de leitura.

---

## 📁 Estrutura do Projeto (Harmony v3)

```text
├── app.c                   # Máquina de estados da aplicação, eventos USB CDC e lógica de I/O
├── app.h                   # Definições de estruturas, protótipos e estados da aplicação
├── config/                 # Arquivos de configuração gerados pelo MPLAB Harmony Configurator (MCC)
└── README.md               # Documentação do projeto
\_OUT \_\[Atualizar com o pino correto, ex: RA0]\_ -> Conectado ao sinal do Servo



\## 🚀 Como clonar e compilar

1\. Clone o repositório em sua pasta de projetos:

&#x20;  ```bash

&#x20;  git clone https://github.com

&#x20;  ```

2\. Abra o MPLAB X IDE.

3\. Vá em \*\*File > Open Project\*\* e selecione a pasta `GoHobby\_Servo.X`.

4\. Certifique-se de ter o compilador XC8 instalado e configurado nas propriedades do projeto.

5\. Clique em \*\*Clean and Build\*\*.



