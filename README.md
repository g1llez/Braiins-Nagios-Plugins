# Braiins Monitoring Plugins

Ce dossier contient les plugins Nagios spécifiquement développés pour le monitoring des pools et machines Braiins.

## 📁 Structure

```
braiins/
├── README.md                    # 📄 Documentation principale
├── pool/                        # 🌊 Plugins Pool Braiins
│   ├── README.md               # 📄 Documentation pool
│   ├── check_pool_hashrate     # 🔍 Hashrate & Shares
│   ├── check_pool_reward       # 🪙 Rewards FPPS
│   ├── check_pool_daily_reward # 📅 Daily Rewards
│   └── check_pool_fpps         # 📊 FPPS Rate
└── machine/                     # ⚙️ Plugins Machine
    ├── README.md               # 📄 Documentation machine
    ├── check_machine_hashrate  # 🔍 Hashrate machine
    ├── check_machine_power     # ⚡ Power consumption
    ├── check_machine_efficiency # ⚡ Efficiency (J/TH)
    ├── check_braiins_efficiency # ⚡ Efficiency (sats/W/H combined)
    ├── check_machine_temp_boards # 🌡️ Board temperature
    └── check_machine_temp_chips  # 🌡️ Chip temperature
```

## 🔧 Configuration requise

### Dépendances communes
- `curl` - Pour les requêtes API
- `jq` - Pour le parsing JSON
- `bc` - Pour les calculs de précision
- `grpcurl` - Pour les requêtes gRPC (machine)

### Variables d'environnement
- `BRAIINS_API_KEY` - Clé API Braiins Pool (optionnel, peut être passée en paramètre)

## 🚀 Installation

1. Copier les plugins dans le dossier `customplugins/braiins/`
2. Rendre les scripts exécutables : `chmod +x pool/* machine/*`
3. Configurer les commandes dans `etc/objects/commands.cfg`
4. Configurer les services dans `etc/objects/antminer.cfg`

## 📝 Configuration Nagios

### Commande (etc/objects/commands.cfg)
```nagios
# Pool plugins
define command{
        command_name    check_pool_hashrate
        command_line    $USER2$/braiins/pool/check_pool_hashrate $ARG1$ $ARG2$ $ARG3$ $ARG4$
        }

define command{
        command_name    check_pool_reward
        command_line    $USER2$/braiins/pool/check_pool_reward $ARG1$ $ARG2$ $ARG3$
        }

define command{
        command_name    check_pool_daily_reward
        command_line    $USER2$/braiins/pool/check_pool_daily_reward $ARG1$ $ARG2$ $ARG3$
        }

define command{
        command_name    check_pool_fpps
        command_line    $USER2$/braiins/pool/check_pool_fpps $ARG1$ $ARG2$ $ARG3$
        }

# Machine plugins
define command{
        command_name    check_machine_hashrate
        command_line    $USER2$/braiins/machine/check_machine_hashrate $HOSTADDRESS$ $ARG1$ $ARG2$ $ARG3$ $ARG4$
        }

define command{
        command_name    check_machine_power
        command_line    $USER2$/braiins/machine/check_machine_power $HOSTADDRESS$ $ARG1$ $ARG2$ $ARG3$ $ARG4$
        }

define command{
        command_name    check_machine_efficiency
    ├── check_braiins_efficiency # ⚡ Efficiency (sats/W/H combined)
        command_line    $USER2$/braiins/machine/check_machine_efficiency $HOSTADDRESS$ $ARG1$ $ARG2$ $ARG3$ $ARG4$
    ├── check_braiins_efficiency # ⚡ Efficiency (sats/W/H combined)
        }

define command{
        command_name    check_machine_temp_boards
        command_line    $USER2$/braiins/machine/check_machine_temp_boards $HOSTADDRESS$ $ARG1$ $ARG2$ $ARG3$ $ARG4$
        }

define command{
        command_name    check_machine_temp_chips
        command_line    $USER2$/braiins/machine/check_machine_temp_chips $HOSTADDRESS$ $ARG1$ $ARG2$ $ARG3$ $ARG4$
        }

# Combined efficiency
define command{
        command_name    check_braiins_efficiency
        command_line    $USER2$/braiins/machine/check_braiins_efficiency $HOSTADDRESS$ $ARG1$ $ARG2$ $ARG3$ $ARG4$ $ARG5$
        }
```

## 🔗 Repository GitHub

Ces plugins sont maintenus dans le repository GitHub :
- **Repository** : g1llez/Braiins-Nagios-Plugins(https://github.com/g1llez/Braiins-Nagios-Plugins)
- **License** : Apache-2.0
- **Issues : https://github.com/g1llez/Braiins-Nagios-Plugins/issues)

## 🤝 Contribution

Les contributions sont les bienvenues ! Veuillez :
1. Fork le repository
2. Créer une branche pour votre fonctionnalité
3. Commiter vos changements
4. Pousser vers la branche
5. Créer une Pull Request

## 📞 Support

Pour toute question ou problème :
- Ouvrir une issue sur GitHub
- Contacter : gauclair@sarius.ca

## 📄 License

Apache-2.0

