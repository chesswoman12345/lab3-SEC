# 🔐 Android Emulator + Burp Suite Lab
## Observation et Analyse du Trafic Réseau Mobile en Environnement Contrôlé

---

# 📌 Présentation du Lab

Ce laboratoire met en place un environnement d’analyse réseau entre un **Android Emulator** et une **application cible autorisée** à travers **Burp Suite Community Edition** agissant comme proxy d’observation.

L’objectif principal est de comprendre :

- Comment un proxy intercepte le trafic réseau ;
- Ce que Burp Suite peut capturer ;
- Comment analyser une requête HTTP/HTTPS ;
- Comment documenter correctement des observations de sécurité ;
- Les bonnes pratiques de sécurité liées à l’interception réseau en laboratoire.

Ce lab est destiné à un usage **strictement pédagogique et autorisé**.

---

# 🎯 Objectifs Pédagogiques

À la fin du laboratoire, il sera possible de :

- Vérifier qu’un Android Emulator envoie correctement son trafic via Burp Suite ;
- Comprendre le rôle d’un proxy dans une communication réseau ;
- Identifier les composants essentiels d’une requête HTTP ;
- Différencier HTTP et HTTPS ;
- Comprendre le rôle d’un certificat CA dans un environnement de test ;
- Produire une trace d’audit simple et reproductible.

---

# 🛠️ Environnement de Travail

## Logiciels requis

- Burp Suite Community Edition
- Android Studio Emulator (Pixel / Nexus recommandé)
- Navigateur Android intégré ou navigateur de test
- Réseau de laboratoire isolé
- Application ou site de test autorisé

---

# ⚠️ Règles de Sécurité Obligatoires

## ✅ Autorisé

- Interception uniquement sur des cibles autorisées ;
- Utilisation d’environnements de test ;
- Utilisation de comptes fictifs ou de démonstration.

## ❌ Interdit

- Utilisation de comptes personnels ;
- Interception de trafic tiers ;
- Utilisation hors environnement pédagogique ;
- Conservation du certificat CA après le laboratoire.

---

# 🧩 Architecture du Lab

```text
┌─────────────────────┐
│ Android Emulator    │
│ (Navigateur/App)    │
└─────────┬───────────┘
          │
          │ Proxy Manual
          ▼
┌─────────────────────┐
│ Burp Suite Proxy    │
│ <IP_HOTE>:<PORT>    │
└─────────┬───────────┘
          │
          ▼
┌─────────────────────┐
│ Cible Autorisée     │
│ Site/App de Test    │
└─────────────────────┘
```

---

# 🚀 Étape 1 — Préparer Burp Suite

## Procédure

1. Lancer Burp Suite ;
2. Créer un projet temporaire ;
3. Ouvrir l’onglet **Proxy** ;
4. Vérifier la présence de :
   - Intercept
   - HTTP history
5. Vérifier que :

```text
Intercept is off
```

---

## 📸 Captures

<p align="center">
  <img src="./screen 1.png" width="700">
</p>

<p align="center">
  <img src="./scrr 2.png" width="700">
</p>

---

## 🎯 Pourquoi ?

Le proxy doit être correctement configuré avant d’intercepter le trafic.

---

## ❌ Erreurs fréquentes

- Interception activée trop tôt ;
- Navigation bloquée ;
- Mauvais onglet utilisé.

---

# 🌐 Étape 2 — Vérifier le Proxy Listener

## Procédure

1. Aller dans :

```text
Proxy → Options / Proxy Settings
```

2. Vérifier qu’un listener est :
   - Enabled
   - Running

3. Noter :

```text
<PORT_PROXY>
<ADRESSE_LISTENER>
```

Exemple :

```text
127.0.0.1:8080
```

---

## 📸 Captures

<p align="center">
  <img src="./scree 3.png" width="700">
</p>

<p align="center">
  <img src="./scree 4.png" width="700">
</p>

---

## 🎯 Pourquoi ?

Le listener représente le point d’entrée réseau du proxy.

---

## ❌ Erreurs fréquentes

- Listener désactivé ;
- Mauvais port ;
- Loopback inaccessible depuis l’émulateur.

---

# 🖥️ Étape 3 — Identifier l’Adresse IP de l’Hôte

## Procédure

### Windows

```bash
ipconfig
```

### Linux / macOS

```bash
ifconfig
```

ou

```bash
ip addr
```

---

## 📸 Capture

<p align="center">
  <img src="./scree 7.png" width="700">
</p>

---

## 🎯 Pourquoi ?

L’émulateur doit connaître l’adresse du proxy.

---

## ❌ Erreurs fréquentes

- Utiliser une IP VPN ;
- Confondre IP publique et IP locale ;
- Interface réseau inactive.

---

# 📱 Étape 4 — Configurer le Proxy Android

## Procédure

Dans l’Android Emulator :

```text
Settings → Wi-Fi → Réseau Actif
```

Puis :

```text
Advanced Options → Proxy → Manual
```

Configurer :

```text
Proxy hostname : <IP_HOTE>
Proxy port     : <PORT_PROXY>
```

---

## 📸 Captures

<p align="center">
  <img src="./scree 8.png" width="250">
  <img src="./screen 12.png" width="250">
  <img src="./screen 14.png" width="250">
</p>

---

## 🎯 Pourquoi ?

Sans proxy configuré, le trafic contourne Burp Suite.

---

## ❌ Erreurs fréquentes

- Mauvais réseau Wi-Fi ;
- Port incorrect ;
- Hostname vide.

---

# 🌍 Étape 5 — Premier Test HTTP

## Procédure

1. Ouvrir le navigateur Android ;
2. Accéder à une cible autorisée ;
3. Retourner dans Burp ;
4. Ouvrir :

```text
HTTP history
```

---

## 📸 Captures

<p align="center">
  <img src="./screen 9.png" width="300">
  <img src="./scren 10.png" width="600">
</p>

---

## 🎯 Pourquoi ?

Valider que la chaîne proxy fonctionne.

---

## ❌ Erreurs fréquentes

- Aucun trafic visible ;
- Proxy mal configuré ;
- Listener inaccessible.

---

# 🔍 Étape 6 — Lire une Requête HTTP

## Procédure

Sélectionner une requête dans :

```text
HTTP history
```

Observer :

- Méthode HTTP ;
- URL ;
- Headers ;
- Cookies ;
- Paramètres.

---

## 📸 Captures

<p align="center">
  <img src="./screen 11.png" width="600">
</p>

<p align="center">
  <img src="./screen 13.png" width="600">
</p>

---

## 🎯 Pourquoi ?

L’analyse réseau consiste à comprendre les données échangées.

---

## ✅ Méthodes HTTP fréquentes

```http
GET
POST
PUT
DELETE
```

---

## ✅ Headers importants

```http
User-Agent
Cookie
Authorization
Accept
Content-Type
```

---

## ❌ Erreurs fréquentes

- Ignorer les headers ;
- Confondre cookies et mots de passe ;
- Analyser sans contexte.

---

# 🧠 Étape 7 — Interception Contrôlée

## Procédure

1. Activer :

```text
Intercept is on
```

2. Rafraîchir une page ;
3. Observer la requête bloquée ;
4. Désactiver immédiatement l’interception.

---

## 📸 Capture

<p align="center">
  <img src="./scree 15.png" width="700">
</p>

---

## 🎯 Pourquoi ?

Comprendre le rôle actif du proxy.

---

## ❌ Erreurs fréquentes

- Oublier de désactiver l’interception ;
- Bloquer tout le trafic.

---

# 🔐 Étape 8 — HTTPS et Certificat CA

## 📌 Concept

HTTPS chiffre les communications.

Un proxy d’analyse HTTPS nécessite un certificat CA de laboratoire afin que le navigateur fasse confiance au proxy.

---

## 📸 Captures

<p align="center">
  <img src="./scree 17.png" width="250">
  <img src="./scree 18.png" width="250">
</p>

---

## ⚠️ Important

Le certificat CA :

- doit rester temporaire ;
- doit être utilisé uniquement en laboratoire ;
- doit être supprimé à la fin.

---

## ❌ Erreurs fréquentes

- Installer sur téléphone personnel ;
- Oublier la suppression.

---

# 📝 Étape 9 — Mini Rapport d’Audit

## Contenu attendu

### 1. Périmètre

```text
Android Emulator + cible autorisée
```

### 2. Configuration

```text
Burp Version
IP Hôte
Port Proxy
Date
```

### 3. Preuves

- Capture HTTP history ;
- Détails d’une requête ;
- Headers observés.

### 4. Analyse

Exemples :

- paramètres visibles ;
- cookies présents ;
- tokens dans URL ;
- informations sensibles potentielles.

### 5. Recommandations

- minimiser les données ;
- sécuriser les cookies ;
- utiliser HTTPS ;
- appliquer les bonnes pratiques Android.

---

# 📋 Checkpoints de Validation

## ✅ Validation Technique

- [ ] Burp capture des requêtes ;
- [ ] Listener actif ;
- [ ] Proxy Android configuré ;
- [ ] HTTP history fonctionnel ;
- [ ] Intercept testé puis désactivé.

---

## ✅ Validation Documentation

- [ ] Rapport produit ;
- [ ] Captures contextualisées ;
- [ ] Paramètres documentés ;
- [ ] Recommandations rédigées.

---

# 🧹 Nettoyage de Fin de Lab

## Obligatoire

- Désactiver le proxy Android ;
- Supprimer le certificat CA ;
- Fermer Burp Suite ;
- Restaurer l’environnement initial.

---

# 🔒 Bonnes Pratiques de Sécurité

## Toujours

- travailler dans un environnement isolé ;
- documenter le contexte ;
- utiliser des cibles autorisées ;
- limiter les permissions.

## Jamais

- intercepter du trafic réel non autorisé ;
- conserver des certificats de test ;
- manipuler des comptes personnels.

---

# 📚 Concepts Clés Retenus

| Concept | Description |
|---|---|
| Proxy | Intermédiaire réseau |
| HTTP | Trafic non chiffré |
| HTTPS | Trafic chiffré |
| CA Certificate | Autorité de confiance |
| Intercept | Blocage temporaire des requêtes |
| HTTP History | Journal des échanges réseau |

---

# 🎓 Conclusion

Ce laboratoire introduit les bases de l’analyse réseau mobile à travers Burp Suite et Android Emulator dans un cadre pédagogique sécurisé.

L’objectif principal n’est pas la modification du trafic, mais :

- la compréhension du fonctionnement réseau ;
- l’observation des échanges HTTP/HTTPS ;
- l’apprentissage des bonnes pratiques d’audit ;
- la production de documentation claire et reproductible.

Une bonne pratique de cybersécurité repose autant sur la rigueur technique que sur la qualité de la documentation produite.

---

# 📖 Auteur / Informations

```text
Lab : Android Emulator + Burp Suite
Type : Observation Réseau Mobile
Niveau : Débutant / Intermédiaire
Objectif : Analyse et documentation réseau
Environnement : Contrôlé et autorisé
```
