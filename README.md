# 🔐 Android Emulator + Burp Suite Lab  
## Observation et Analyse du Trafic Réseau Mobile en Environnement Contrôlé

---

## 📌 Présentation du Lab

Ce laboratoire met en place un environnement d’analyse réseau entre un **Android Emulator** et une **application cible autorisée** à travers **Burp Suite Community Edition** agissant comme proxy d’observation.

L’objectif principal est de comprendre :

- comment un proxy intercepte le trafic réseau ;
- ce que Burp Suite peut capturer ;
- comment analyser une requête HTTP/HTTPS ;
- comment documenter correctement des observations de sécurité ;
- les bonnes pratiques de sécurité liées à l’interception réseau en laboratoire.

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

- **Burp Suite Community Edition**
- **Android Studio Emulator** (Pixel / Nexus recommandé)
- Navigateur Android intégré ou navigateur de test
- Réseau de laboratoire isolé
- Application ou site de test autorisé

---

# ⚠️ Règles de Sécurité Obligatoires

## Ce qui est autorisé

- Interception uniquement sur des cibles autorisées ;
- Utilisation d’environnements de test ;
- Utilisation de comptes fictifs ou de démonstration.

## Ce qui est interdit

- Utilisation de comptes personnels ;
- Interception de trafic tiers ;
- Utilisation hors environnement pédagogique ;
- Conservation du certificat CA après le laboratoire.

---

# 🖼️ Captures du Lab

Ajoute les images dans un dossier :

```text
/images
```

Structure recommandée :

```text
project/
│
├── README.md
└── images/
    ├── screen1.png
    ├── screen2.png
    ├── screen3.png
    └── ...
```

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
   - **Intercept**
   - **HTTP history**
5. Vérifier que :
   ```text
   Intercept is off
   ```

---

## 📸 Capture

```markdown
![Préparation Burp](images/screen1.png)
```

```markdown
![Proxy Intercept](images/screen2.png)
```

---

## 🎯 Pourquoi ?

Le proxy doit d’abord être configuré correctement avant d’intercepter le trafic.

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

---

## 📸 Capture

```markdown
![Proxy Listener](images/screen3.png)
```

```markdown
![Listener Configuration](images/screen4.png)
```

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

```markdown
![Adresse IP Host](images/screen7.png)
```

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

```markdown
![Configuration WiFi](images/screen8.png)
```

```markdown
![Proxy Android](images/screen12.png)
```

```markdown
![Advanced Network](images/screen14.png)
```

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

```markdown
![Navigateur Android](images/screen9.png)
```

```markdown
![HTTP History](images/screen10.png)
```

---

# 🔍 Étape 6 — Lire une Requête HTTP

## Procédure

Sélectionner une requête dans :

```text
HTTP history
```

Observer :

- Méthode HTTP
- URL
- Headers
- Cookies
- Paramètres

---

## 📸 Captures

```markdown
![Analyse Requête](images/screen11.png)
```

```markdown
![Request Inspector](images/screen13.png)
```

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

```markdown
![Intercept Request](images/screen15.png)
```

---

# 🔐 Étape 8 — HTTPS et Certificat CA

## 📌 Concept

HTTPS chiffre les communications.

Un proxy d’analyse HTTPS nécessite un certificat CA de laboratoire afin que le navigateur fasse confiance au proxy.

---

## 📸 Captures

```markdown
![Encryption Credentials](images/screen17.png)
```

```markdown
![Install Certificate](images/screen18.png)
```

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
```
