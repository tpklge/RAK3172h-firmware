# Meshtastic para RAK3172H + Cardputer via Grove

Firmware experimental baseado no Meshtastic **2.7.26** para utilizar um
**RAK3172H** como rádio externo de um M5Stack Cardputer/MeshClient através da
UART do conector Grove.

Esta é a terceira revisão do firmware e foi a versão validada em hardware real.

## O que esta versão faz

- usa a UART1 do RAK3172H no conector Grove;
- `PB6` como TX e `PB7` como RX;
- ativa o módulo Serial do Meshtastic em modo `PROTO`;
- configura a UART para `115200 bit/s`;
- aplica `ANZ` como região padrão em instalações novas;
- força a configuração da UART também em nós que já possuem preferências gravadas;
- preserva Node ID, canais e demais configurações existentes quando o firmware é
  gravado sem apagar toda a flash.

> Este firmware não converte hardware de 433 MHz em 915 MHz. Ele foi preparado
> para o RAK3172H US915 utilizado no projeto.

## Base do código

- Projeto original: <https://github.com/meshtastic/firmware>
- Versão: `2.7.26`
- Commit base: `54e0d8d0ab2ff56b3a9ce967e53f79e49af560fb`
- Ambiente PlatformIO: `rak3172`

O README original do Meshtastic foi preservado em
[`README_UPSTREAM.md`](README_UPSTREAM.md).

## Arquivos modificados

- `variants/stm32/rak3172/platformio.ini`
- `src/mesh/NodeDB.cpp`
- `src/modules/Modules.cpp`

O patch completo está em
[`patches/rak3172h-cardputer-grove-proto-anz.patch`](patches/rak3172h-cardputer-grove-proto-anz.patch).

## Compilação no VS Code

1. Instale a extensão **PlatformIO IDE**.
2. Abra esta pasta como projeto no VS Code.
3. Aguarde o PlatformIO instalar as dependências.
4. Execute no terminal do PlatformIO:

```bash
pio run -e rak3172
```

O binário será gerado em:

```text
.pio/build/rak3172/firmware.bin
```

## Binário validado

O firmware utilizado no teste bem-sucedido está em:

```text
release/firmware-rak3172-2.7.26-meshclient-grove-proto-anz.bin
```

```text
MD5     fd41083330e7478fca5baa3e8a5d2654
SHA-256 9917e3ae23c2d3db8664ab90384d32160aeea76b5f140537ffe0e777e8805c8d
```

## Gravação por ST-Link

No STM32CubeProgrammer:

1. conecte `SWDIO`, `SWCLK`, `GND` e a referência de `3V3` corretamente;
2. selecione a interface SWD;
3. carregue o arquivo `.bin` da pasta `release`;
4. use o endereço inicial `0x08000000`;
5. grave e verifique o conteúdo.

Para manter Node ID, canais e configurações, **não execute Full Chip Erase**.
Faça apagamento completo somente quando realmente desejar restaurar todo o nó.

## UART entre RAK3172H e Cardputer

A comunicação utiliza `115200 8N1`, sem controle de fluxo:

| RAK3172H | Cardputer | Função |
|---|---|---|
| `PB6 / U1TX` | RX | Dados do RAK para o Cardputer |
| `PB7 / U1RX` | TX | Dados do Cardputer para o RAK |
| GND | GND | Referência comum |

TX e RX devem ser cruzados. O transporte usado é o protocolo serial protobuf do
Meshtastic (`PROTO`), não o console de texto.

## Observação sobre ANZ915

A flag compilada define `ANZ` como região inicial. Se o dispositivo já possuir
uma região gravada, ela é preservada. A região pode ser conferida ou alterada
posteriormente pelas ferramentas Meshtastic.

O uso de radiofrequência deve respeitar as regras aplicáveis no local de operação.

## Licença e créditos

Este repositório deriva do firmware Meshtastic e mantém sua licença GPL-3.0.
Consulte [`LICENSE`](LICENSE) e os avisos existentes no código-fonte. Meshtastic
é um projeto de seus respectivos mantenedores e colaboradores.

## Modificações para RAK3172H e Cardputer

As adaptações para comunicação UART/Grove, configuração da região ANZ
e suporte ao MeshClient foram desenvolvidas por:

Copyright © 2026 Tiago Paludo

Este projeto é derivado do firmware Meshtastic e permanece licenciado
sob a GNU General Public License v3.0.
