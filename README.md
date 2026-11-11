# WifiSense

Detecção de movimento por WiFi (CSI) com ESP32 DevKit V1, publicando no HiveMQ Cloud e exibindo num painel web para celular.

```
ESP32 (lê CSI do roteador, calcula movimento no chip)
  → MQTT/TLS (8883) → HiveMQ Cloud → painel (MQTT/WebSocket 8884) no celular
```

- `firmware/wifisense/` – sketch para Arduino IDE
- `docs/index.html` – painel (publique com GitHub Pages: Settings → Pages → branch `main`, pasta `/docs`)
- `GUIA.md` – passo a passo completo

**Limites:** detecta movimento (não pessoa parada), com calibração por ambiente. Não é dispositivo médico nem de segurança. Use só em locais seus ou com autorização e avise quem é monitorado.
**Segurança:** o firmware usa `setInsecure()` (não valida o certificado do HiveMQ). Para produção, embuta o certificado raiz ISRG Root X1 e use `tls.setCACert()`.
