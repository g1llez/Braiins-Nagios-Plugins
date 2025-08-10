# Braiins Pool Plugins

Ce dossier contient les plugins Nagios pour le monitoring des pools Braiins.

## 📁 Plugins disponibles

### `check_pool_hashrate`
- **Description** : Vérifie le hashrate et les shares des pools Braiins
- **Usage** : `check_pool_hashrate <api_key> <metric_type> <warning_threshold> <critical_threshold>`
- **Métriques** : `hashrate`, `shares`
- **Période** : 60 minutes pour les seuils

### `check_pool_reward`
- **Description** : Vérifie les récompenses FPPS des pools Braiins
- **Usage** : `check_pool_reward <api_key> <warning_threshold> <critical_threshold>`
- **Unité** : sats

### `check_pool_daily_reward`
- **Description** : Vérifie les récompenses quotidiennes des pools Braiins
- **Usage** : `check_pool_daily_reward <api_key> <warning_threshold> <critical_threshold>`
- **Unité** : sats

### `check_pool_fpps`
- **Description** : Vérifie le taux FPPS des pools Braiins
- **Usage** : `check_pool_fpps <api_key> <warning_threshold> <critical_threshold>`

## 🔧 Configuration requise

### Dépendances
- `curl` - Pour les requêtes API
- `jq` - Pour le parsing JSON
- `bc` - Pour les calculs de précision

### Variables d'environnement
- `BRAIINS_API_KEY` - Clé API Braiins Pool (optionnel, peut être passée en paramètre)

## 📊 Seuils recommandés

### S21 (Antminer S21 Pro/S21)
- **Hashrate** : Warning 185, Critical 170 TH/s
- **Shares** : Warning 140M, Critical 120M shares
- **Reward** : Warning 350, Critical 330 sats
- **Daily Reward** : Warning 9800, Critical 9500 sats

### S19 (Antminer S19)
- **Hashrate** : Warning 100, Critical 95 TH/s
- **Shares** : Warning 500, Critical 400
- **Reward** : Warning 200, Critical 190 sats
- **Daily Reward** : Warning 4900, Critical 4750 sats
