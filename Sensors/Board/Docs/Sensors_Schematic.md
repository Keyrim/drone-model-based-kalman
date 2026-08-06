# Schéma Sensors (« Gyroscope »)

Description du schéma [`Sensors_Schematic.pdf`](Sensors_Schematic.pdf) (export du `Sensors.SchDoc`, 1 feuille A4).

Carte capteurs du drone Mark 1 : une centrale inertielle **MPU-6000** (gyroscope + accéléromètre 6 axes) et un baromètre **MS5611**, reliés à la carte Flight_Controller par un unique connecteur JST-SH 8 points. L'IMU est sur le bus SPI3, le baromètre sur le bus I2C1 (les pull-ups I2C sont côté Flight_Controller).

## MPU-6000 (MPUA) — IMU 6 axes, mode SPI

| Broche | Câblage |
|---|---|
| SCL/SCLK (23) | SPI3_SCK |
| SDA/SDI (24) | SPI3_MOSI |
| AD0/SDO (9) | SPI3_MISO |
| CS (8) | MPU_CS |
| CLKIN (1) | GND (horloge interne) |
| FSYNC (11) | GND |
| INT (12) | Non connectée — pas de ligne d'interruption vers le MCU |
| VDD (13) | 3V3, découplage C3 100 nF |
| REGOUT (10) | C1 100 nF vers GND |
| CPOUT (20) | C2 2,2 nF vers GND |
| GND (18) / EP (25) | GND |
| AUX_CL / AUX_DA (7/6) | Non connectées (pas de capteur auxiliaire) |

Le second symbole `MPUB` regroupe uniquement les broches NC du boîtier — ce n'est pas un deuxième composant.

## MS5611-01BA03 — baromètre, mode I2C

| Broche | Câblage |
|---|---|
| PS (2) | VDD (3V3) → protocole I2C sélectionné |
| CSB (4/5) | GND → adresse I2C 0x77 |
| SDI/SDA (7) | I2C1_SDA |
| SCLK (8) | I2C1_SCL |
| SDO (6) | Non connectée (inutilisée en I2C) |
| VDD (1) | 3V3, découplage C4 100 nF |

## Connecteur `SensorBoard` — SM08B-SRSS-TB (8 pts)

Brochage identique au connecteur `sensor_board` de la carte Flight_Controller (liaison 1:1) :

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
