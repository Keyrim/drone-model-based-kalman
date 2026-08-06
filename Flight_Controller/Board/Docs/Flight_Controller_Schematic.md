# Schéma Flight_Controller

Description du schéma [`Flight_Controller_Schematic.pdf`](Flight_Controller_Schematic.pdf) (export du `Flight_Controller.SchDoc`, 1 feuille A3).

Carte contrôleur de vol du drone Mark 1. Elle héberge le microcontrôleur et sert de hub : les moteurs (via la carte Power), les capteurs (carte Sensors), les ESC, un GPS optionnel et trois UART d'extension s'y raccordent par connecteurs JST-SH.

## MCU — STM32F405RGT6 (LQFP64)

- **Quartz** Y1 8 MHz (QTC5A8.00000FDT1I20R) sur PH0/PH1, condensateurs de charge C16/C17 17 pF.
- **Reset** : NRST tiré à 3V3 par Rnrst 100 kΩ + condensateur c15 100 nF vers GND, relié au header de programmation (`PROG_NRST`).
- **BOOT0** câblé à la masse (boot flash uniquement).
- **Alimentation** : VBAT sur 3V3, VCAP_1/VCAP_2 sur 2,2 µF (CVCAP1/CVCAP2), VDDA/VSSA sur 3V3/GND.

### Affectation des broches

| Fonction | Broches | Va vers |
|---|---|---|
| UART1 | PB6 (TX), PB7 (RX) | Connecteur `uart1` |
| UART2 RX seul | PA3 | Connecteur `esc` (pin 8) — télémétrie ESC |
| UART3 | PB10 (TX), PB11 (RX) | Connecteur `uart3` |
| UART4 | PA0 (TX), PA1 (RX) | Connecteur `uart4` |
| UART6 | PC6 (TX), PC7 (RX) | Connecteur `GPS?` |
| I2C1 | PB8 (SCL), PB9 (SDA) | `sensor_board` + `GPS?` — pull-ups R3/R4 4,7 kΩ vers 3V3 |
| SPI3 | PC10 (SCK), PC11 (MISO), PC12 (MOSI) + PA15 (`MPU_CS`) | `sensor_board` (IMU MPU-6000) |
| TIM1 CH1–CH4 | PA8, PA9, PA10, PA11 | `power_board` — PWM moteurs |
| TIM3 CH1–CH4 | PA6, PA7, PB0, PB1 | `esc` — PWM ESC |
| ADC1_IN13 | PC3 | `power_board` — mesure analogique (tension batterie) |
| ADC1_IN14 | PC4 | `esc` — mesure analogique |
| GPIO | PC9 | `power_board` (pin 7) |
| LED1 / LED2 | PC13 / PC14 | LEDs de statut, résistances série 150 Ω |
| SWD | PA13 (SWDIO), PA14 (SWCLK), PB3 (SWO) | Header `Prog` |

Broches laissées non connectées (croix rouges) : PA2, PA4, PA5, PA12, PB2, PB4, PB5, PB12–PB15, PC0–PC2, PC5, PC8, PC15, PD2.

## Alimentation

- Entrée **5Vin** en provenance de la carte Power (pin 1 du connecteur `power_board`), diode Schottky série D1 MBR0520LT1G (protection inversion) vers le rail **+5** ; C3 10 µF.
- LDO **MCP1826S-3302E/DB** : +5 → **3V3**, C4 4,7 µF en sortie.
- Le rail +5 alimente aussi les connecteurs `uart1/3/4` et `GPS?`.
- Découplage 3V3 : 5 × 100 nF (C5–C9) et 5 × 1 µF (C10–C14).
- LED_PWR sur 3V3 avec résistance 150 Ω (témoin d'alimentation).

## Connecteurs

Tous les connecteurs signaux sont des JST-SH (séries SM04B/SM08B-SRSS-TB et SM06B-GHS-TB).

### `uart1`, `uart3`, `uart4` — SM04B-SRSS-TB (4 pts)

| Pin | Signal |
|---|---|
| 1 | GND |
| 2 | +5 |
| 3 | UARTx_RX |
| 4 | UARTx_TX |

### `power_board` — SM08B-SRSS-TB (8 pts)

| Pin | Signal |
|---|---|
| 1 | 5Vin (entrée alim) |
| 2 | GND |
| 3 | TIM1_CH4 |
| 4 | TIM1_CH3 |
| 5 | TIM1_CH2 |
| 6 | TIM1_CH1 |
| 7 | PC9 |
| 8 | ADC1_IN13 |

### `sensor_board` — SM08B-SRSS-TB (8 pts)

| Pin | Signal |
|---|---|
| 1 | 3V3 |
| 2 | MPU_CS |
| 3 | SPI3_SCK |
| 4 | SPI3_MISO |
| 5 | SPI3_MOSI |
| 6 | I2C1_SCL |
| 7 | I2C1_SDA |
| 8 | GND |

### `esc` — SM08B-SRSS-TB (8 pts)

| Pin | Signal |
|---|---|
| 1 | GND |
| 2 | GND |
| 3 | TIM3_CH4 |
| 4 | TIM3_CH3 |
| 5 | TIM3_CH2 |
| 6 | TIM3_CH1 |
| 7 | ADC1_IN14 |
| 8 | UART2_RX |

### `GPS?` — SM06B-GHS-TB (6 pts)

| Pin | Signal |
|---|---|
| 1 | GND |
| 2 | I2C1_SDA |
| 3 | I2C1_SCL |
| 4 | UART6_RX |
| 5 | UART6_TX |
| 6 | +5 |

### `Prog` — header SWD 6 pts (Würth 61300611021)

| Pin | Signal |
|---|---|
| 1 | 3V3 |
| 2 | PROG_SWCLK |
| 3 | GND |
| 4 | PROG_SWDIO |
| 5 | PROG_NRST |
| 6 | PROG_SWO |
