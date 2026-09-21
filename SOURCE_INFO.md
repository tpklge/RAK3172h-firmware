# Informações de reprodução

Este pacote representa a terceira revisão do firmware RAK3172H/Cardputer.

- Upstream: `https://github.com/meshtastic/firmware.git`
- Commit: `54e0d8d0ab2ff56b3a9ce967e53f79e49af560fb`
- Versão reportada: `v2.7.26.54e0d8d`
- Submódulo `protobufs`: `6b1ded439633cd03d4af85b44231b91d1d106278`
- Submódulo `meshtestic`: `dcac7e5673005f4d8a2b1f0f6e06877b689d7519`
- Ambiente: `rak3172`
- Destino de gravação: `0x08000000`

## Alterações funcionais

1. `RAK3172_MESHCLIENT_GROVE` delimita as mudanças específicas deste projeto.
2. A UART1 usa `PB7` para RX e `PB6` para TX.
3. `SERIAL_PRINT_PORT=1` mantém a porta serial destinada ao transporte.
4. O módulo Serial é configurado como `PROTO`, `115200`, timeout `250`.
5. A configuração é aplicada tanto no primeiro boot quanto durante a criação
   dos módulos, cobrindo dispositivos que já tinham preferências persistidas.
6. `USERPREFS_CONFIG_LORA_REGION` define `ANZ` como padrão sem sobrescrever
   configurações persistidas.

O patch reproduzível está na pasta `patches`.

## Verificação de compilação

O código deste pacote foi compilado do zero com sucesso em 21 de setembro de
2026, usando PlatformIO e o ambiente `rak3172`:

```sh
pio run -e rak3172
```

Resultado: 21.448 bytes de RAM (32,7%) e 173.088 bytes de flash (74,1%).

O arquivo da pasta `release` é o terceiro firmware efetivamente testado no
hardware e foi mantido sem alterações. Uma recompilação pode produzir hash
diferente por incorporar metadados da compilação, embora use o mesmo código.
