# 🕐 Montre Connectée — nRF5340DK

> Projet académique — Master 1 TDSI Objets Connectés | Université de Poitiers

Implémentation d'une montre connectée embarquée sur carte **nRF5340DK**, avec affichage LVGL, acquisition multi-capteurs, communication BLE, horodatage RTC et journalisation sur carte SD.

\---

## 📋 Présentation

Ce projet consiste à concevoir un système embarqué temps réel simulant les fonctionnalités d'une montre connectée :

* Affichage d'une interface graphique interactive sur écran TFT tactile
* Acquisition de données environnementales (température, humidité, pression, IMU, magnétomètre)
* Communication Bluetooth Low Energy (BLE) avec un appareil hôte
* Horodatage précis via RTC externe
* Journalisation des données sur carte SD

Le firmware est développé avec **Zephyr RTOS / nRF Connect SDK v3.2.1** et l'interface graphique avec **LVGL 9.3** (UI générée sous SquareLine Studio).

\---

## 🔧 Hardware

|Composant|Référence|Interface|
|-|-|-|
|Microcontrôleur|nRF5340DK (Nordic Semiconductor)|—|
|Shield capteurs|X-NUCLEO-IKS01A3|I²C|
|Capteur T°/Humidité|HTS221|I²C|
|Capteur pression|LPS22HH|I²C|
|IMU (accéléro + gyro)|LSM6DSO|I²C|
|Magnétomètre|LIS2MDL|I²C|
|Écran TFT|Adafruit 2.8" ILI9341|SPI|
|Contrôleur tactile|TSC2007|I²C|
|RTC|RV-8263-C8|I²C|
|Stockage|Carte MicroSD|SPI|

\---

## ⚙️ Installation

### Prérequis

* [nRF Connect SDK v3.2.1](https://developer.nordicsemi.com/nRF_Connect_SDK/doc/3.2.1/nrf/index.html)
* [nRF Connect for Desktop](https://www.nordicsemi.com/Products/Development-tools/nRF-Connect-for-Desktop) + Toolchain Manager
* `west` CLI installé et configuré
* Optionnel : [SquareLine Studio](https://squareline.io/) pour modifier l'UI LVGL

### Cloner le dépôt

```bash
git clone https://github.com/<votre-username>/montre-connectee.git
cd montre-connectee
```

### Compiler

```bash
west build -b nrf5340dk/nrf5340/cpuapp
```

### Flasher

```bash
west flash
```

### Moniteur série (debug)

```bash
west espresso  # ou
minicom -D /dev/ttyACM0 -b 115200
```

\---

## ✨ Fonctionnalités

### Interface graphique

* UI multi-écrans générée avec SquareLine Studio + LVGL 9.3
* Écran principal : heure, date, température, humidité
* Écran BLE : statut de connexion et données reçues
* Navigation tactile via TSC2007

### Acquisition capteurs

* Lecture périodique via threads Zephyr (température, humidité, pression, accélération, magnétisme)
* Synchronisation par sémaphores

### Bluetooth Low Energy (BLE)

* Rôle : périphérique (peripheral)
* Service GATT personnalisé (`peripheral\_bms`)
* Envoi de données capteurs vers un hôte BLE

### Horodatage

* RTC externe RV-8263-C8 via I²C brut (adresse `0x51`)
* Heure affichée et injectée dans les logs

## 📁 Structure du code

```
montre-connectee/
├── src/
│   ├── main.c                  # Point d'entrée, initialisation, threads
│   ├── ble/
│   │   ├── ble\_service.c       # Service GATT personnalisé
│   │   └── ble\_service.h
│   ├── sensors/
│   │   ├── sensors.c           # Lecture HTS221, LPS22HH, LSM6DSO, LIS2MDL
│   │   └── sensors.h
│   ├── rtc/
│   │   ├── rtc.c               # Driver I²C brut RV-8263-C8
│   │   └── rtc.h
│   └── ui/
│       ├── screens/            # Fichiers générés par SquareLine Studio
│       └── ui.c
├── boards/
│   └── nrf5340dk\_nrf5340\_cpuapp.overlay   # Devicetree overlay
├── CMakeLists.txt
├── prj.conf                    # Configuration Kconfig
└── README.md
```

\---

## 👥 Équipe

Projet réalisé dans le cadre du Master 1 TDSI Objets Connectés — Université de Poitiers.

Made With ❤️ By Samuel_DASSI 

## 📄 Licence

Ce projet est à usage académique. Tous droits réservés.

