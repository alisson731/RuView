# Guia passo a passo (Arduino IDE + ESP32 DevKit V1)

## 1. Preparar a Arduino IDE (uma vez só)
1. Instale a **Arduino IDE 2.x** (arduino.cc/en/software).
2. Abra **Arquivo → Preferências**. Em "URLs adicionais para Gerenciadores de Placas", cole:
   `https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json`
3. **Ferramentas → Placa → Gerenciador de Placas**, busque **esp32** (Espressif Systems) e instale. Se der erro de compilação mais adiante, tente a versão 2.0.17.
4. **Ferramentas → Gerenciar Bibliotecas**, instale: **PubSubClient** (Nick O'Leary) e **WiFiManager** (tzapu).
5. Driver USB: se o PC não enxergar a porta, instale o driver **CP2102** ou **CH340** (veja o chip pequeno perto da entrada USB). Use um cabo USB **de dados**.

## 2. HiveMQ Cloud (grátis)
1. Crie conta em hivemq.com/cloud e um cluster gratuito (Serverless).
2. Anote o **host** do cluster (algo como `abc123.s1.eu.hivemq.cloud`).
3. Em **Access Management**, crie 2 credenciais: `esp32` (publicar e assinar) e `painel` (somente assinar; se quiser o botão Recalibrar, permita publicar).

## 3. Gravar o ESP32
1. Abra `firmware/wifisense/wifisense.ino` (a `config.h` abre junto). Edite `MQTT_HOST`, `MQTT_USER`, `MQTT_PASS`.
2. Conecte o ESP32 ao PC. Em **Ferramentas**: Placa **ESP32 Dev Module**, Upload Speed **115200**, Porta (a que aparece ao plugar).
3. Clique em **Upload** (seta →). Quando aparecer `Connecting......`, se travar: **segure o botão BOOT** até começar a gravar, depois solte.
4. Abra **Ferramentas → Monitor Serial** em **115200**. Verá o ID do aparelho e uma linha JSON por segundo.

## 4. Primeira configuração da WiFi
1. No celular, conecte na rede **WifiSense-xxxx** (senha `wifisense123`, em `config.h`). O portal abre sozinho (ou vá a 192.168.4.1).
2. Escolha a WiFi **2,4 GHz** do local, digite a senha e salve. O ESP32 reinicia e conecta.
3. Nos primeiros **20 s** ele calibra: deixe o ambiente **parado**, fora do caminho ESP32↔roteador.

## 5. Painel no celular
1. Crie um repositório **privado** no GitHub, envie estes arquivos, e em **Settings → Pages** escolha branch `main`, pasta `/docs`.
2. Abra o link do Pages no celular, preencha host, usuário `painel` e senha, e toque em Conectar.

## 6. Resetar e regravar
- **Reiniciar:** botão **EN** (ou RST) na placa.
- **Regravar:** só clique **Upload** de novo (segure BOOT se pedir).
- **Trocar de WiFi:** com o ESP32 ligado, **segure BOOT por 3 s**. Ele apaga a WiFi salva e abre de novo a rede `WifiSense-xxxx`.
- **Apagar tudo:** Ferramentas → **Erase All Flash Before Sketch Upload: Enabled** → Upload.

## 7. Dicas de teste
- Ideal: ESP32 a **3 a 8 m** do roteador, com a área de teste entre os dois.
- Falso alarme: aumente `FATOR_LIMIAR`. Não detecta nada: diminua e confira `pacotes/s` (deve ficar bem acima de 20).
- Sem MQTT: confira host, usuário e senha; o monitor serial mostra `MQTT falhou (rc=...)`.
