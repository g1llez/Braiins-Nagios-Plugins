# Braiins Machine Plugins

Ce dossier contient les plugins Nagios pour le monitoring direct des machines Braiins.

## 📁 Plugins disponibles

### `check_machine_hashrate`
- **Description** : Vérifie le hashrate de la machine
- **Usage** : `check_machine_hashrate <host_address> <username> <password> <warning_threshold> <critical_threshold>`
- **Unité** : TH/s

### `check_machine_power`
- **Description** : Vérifie la consommation électrique de la machine
- **Usage** : `check_machine_power <host_address> <username> <password> <warning_threshold> <critical_threshold>`
- **Unité** : W

### `check_machine_efficiency`
- **Description** : Vérifie l'efficacité énergétique de la machine
- **Usage** : `check_machine_efficiency <host_address> <username> <password> <warning_threshold> <critical_threshold>`
- **Unité** : J/TH

### `check_machine_temp_boards`
- **Description** : Vérifie la température des cartes de la machine
- **Usage** : `check_machine_temp_boards <host_address> <username> <password> <warning_threshold> <critical_threshold>`
- **Unité** : °C

### `check_machine_temp_chips`
- **Description** : Vérifie la température des puces de la machine
- **Usage** : `check_machine_temp_chips <host_address> <username> <password> <warning_threshold> <critical_threshold>`
- **Unité** : °C

## 🔧 Configuration requise

### Dépendances
- `grpcurl` - Pour les requêtes gRPC
- `jq` - Pour le parsing JSON
- `bc` - Pour les calculs de précision

### Authentification
- Username et password pour l'accès à la machine
- Accès gRPC sur le port 50051

## 📊 Seuils recommandés

### S21 (Antminer S21 Pro/S21)
- **Hashrate** : Warning 61.7, Critical 56.7 TH/s (par board)
- **Power** : Warning 3800, Critical 3900 W
- **Efficiency** : Warning 18.75, Critical 20 J/TH
- **Temp Boards** : Warning 85, Critical 95°C
- **Temp Chips** : Warning 85, Critical 95°C

### S19 (Antminer S19)
- **Hashrate** : Warning 36.5, Critical 36 TH/s
- **Power** : Warning 3350, Critical 3360 W
- **Efficiency** : Warning 28.5, Critical 29 J/TH
- **Temp Boards** : Warning 80, Critical 90°C
- **Temp Chips** : Warning 80, Critical 90°C
