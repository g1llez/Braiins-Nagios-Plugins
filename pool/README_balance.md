# Plugin check_pool_balance

## 📋 Description

Plugin Nagios pour vérifier le solde des comptes Braiins Pool avec des seuils intelligents basés sur le pourcentage du profit quotidien estimé.

## 🎯 Fonctionnalités

- **Vérification du solde** : Solde disponible, en attente et total
- **Estimation des profits** : Calcul automatique basé sur le hashrate et le FPPS
- **Seuils intelligents** : Alertes basées sur le pourcentage du profit quotidien estimé
- **Conversion automatique** : Affichage en sats et BTC
- **Formatage intelligent** : Utilisation de suffixes K, M, B pour la lisibilité

## 🚀 Usage

```bash
check_pool_balance <api_key> <warning_threshold_percent> <critical_threshold_percent>
```

### Paramètres

- `api_key` : Clé API Braiins Pool
- `warning_threshold_percent` : Seuil d'avertissement en pourcentage du profit quotidien
- `critical_threshold_percent` : Seuil critique en pourcentage du profit quotidien

### Exemples

```bash
# Avertissement si le solde < 80% du profit quotidien, critique si < 50%
check_pool_balance 'your_api_key' 80 50

# Avertissement si le solde < 200% du profit quotidien, critique si < 100%
check_pool_balance 'your_api_key' 200 100
```

## 📊 Logique des seuils

Le plugin calcule automatiquement le profit estimé depuis le dernier retrait basé sur :
- Hashrate 60 minutes de l'utilisateur
- Taux FPPS actuel du pool
- Date du dernier retrait (via l'API payouts)
- Formule : `Hashrate (TH/s) × FPPS (sats/jour/TH) × jours depuis le retrait`

Puis compare le solde total au profit estimé depuis le retrait :
- **OK** : Solde ≥ seuil d'avertissement du profit estimé
- **WARNING** : Solde < seuil d'avertissement du profit estimé
- **CRITICAL** : Solde < seuil critique du profit estimé

### Fallback
Si aucun retrait n'est trouvé dans les 90 derniers jours, le plugin utilise :
- Une date par défaut (30 jours en arrière)
- Ou le profit quotidien estimé comme référence

## 💡 Seuils recommandés

### Configuration conservatrice
- **Warning** : 200% (le solde équivaut à 200% du profit estimé depuis le dernier retrait)
- **Critical** : 100% (le solde équivaut à 100% du profit estimé depuis le dernier retrait)

### Configuration standard
- **Warning** : 100% (le solde équivaut à 100% du profit estimé depuis le dernier retrait)
- **Critical** : 50% (le solde équivaut à 50% du profit estimé depuis le dernier retrait)

### Configuration agressive
- **Warning** : 80% (le solde équivaut à 80% du profit estimé depuis le dernier retrait)
- **Critical** : 30% (le solde équivaut à 30% du profit estimé depuis le dernier retrait)

## 🔧 Dépendances

- `curl` : Requêtes API
- `jq` : Parsing JSON
- `bc` : Calculs de précision

## 📈 Métriques retournées

Le plugin retourne les métriques suivantes pour Nagios :

```
balance_sats=<solde_disponible>
pending_sats=<solde_en_attente>
total_sats=<solde_total>
balance_btc=<solde_disponible_btc>
pending_btc=<solde_en_attente_btc>
total_btc=<solde_total_btc>
estimated_daily_profit=<profit_quotidien_estimé>
estimated_profit_since_withdrawal=<profit_estimé_depuis_le_retrait>
days_since_withdrawal=<jours_depuis_le_retrait>
balance_percent=<pourcentage_du_profit>
```

## 🚨 Gestion des erreurs

- **API inaccessible** : Statut CRITICAL
- **Erreur API** : Statut CRITICAL avec message d'erreur
- **Données manquantes** : Statut UNKNOWN
- **Impossible d'estimer le profit** : Utilisation d'une valeur par défaut (1M sats)

## 🔄 Mise à jour automatique

Le plugin récupère automatiquement :
- Le taux FPPS actuel du pool
- Le hashrate de l'utilisateur
- Les soldes du compte
- La date du dernier retrait (via l'API payouts des 90 derniers jours)

Aucune configuration manuelle des taux ou des dates n'est nécessaire.

## 📝 Configuration Nagios

### Commande

```nagios
define command{
    command_name    check_pool_balance
    command_line    $USER2$/braiins/pool/check_pool_balance $ARG1$ $ARG2$ $ARG3$
}
```

### Service (exemple pour leschatoshis-001)

```nagios
define service{
    use                     generic-service,graphed-service
    host_name               leschatoshis-001
    service_description     Balance Braiins
    check_command           check_pool_balance!$USER101$!100!50
    check_interval          5
    retry_interval          1
    max_check_attempts      3
    notification_interval   30
    notification_period     24x7
    notification_options    w,c,r
    contact_groups          admins
}
```

### Configuration actuelle

Le plugin est configuré sur les pools suivants :
- **leschatoshis-001** : API key `$USER101`, seuils 100%/50%
- **leschatoshis-002** : API key `$USER102`, seuils 100%/50%
- **leschatoshis-003** : API key `$USER103`, seuils 100%/50%

## 🧪 Tests

Utilisez le script de test inclus :

```bash
/var/docker/nagios/tests/test_pool_balance.sh
```

## 🔍 Dépannage

### Vérification des permissions
```bash
chmod +x /var/docker/nagios/customplugins/braiins/pool/check_pool_balance
```

### Test manuel
```bash
/var/docker/nagios/customplugins/braiins/pool/check_pool_balance 'your_api_key' 100 50
```

### Vérification des logs
```bash
tail -f /var/docker/nagios/var/nagios.log | grep "BRAIINS_BALANCE"
```
