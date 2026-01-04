
# 📚 Guide Complet - DeepEduGraph

## 🎯 Vue d'ensemble

**DeepEduGraph** est une application de prédiction du risque d'échec étudiant basée sur l'analyse de graphes et l'intelligence artificielle. Le projet utilise une architecture microservices avec Spring Boot (backend) et React (frontend).

---

## 🏗️ Architecture du Projet

### Structure des Dossiers

```
DeepEduGraph/
├── backend/                    # Services Spring Boot
│   ├── gateway-service/        # API Gateway (port 8080)
│   ├── auth-service/           # Authentification (port 8081)
│   ├── student-service/        # Gestion étudiants (port 8082)
│   ├── course-service/         # Gestion cours (port 8083)
│   ├── tracking-service/       # Suivi activités (port 8084)
│   ├── enrollment-service/     # Inscriptions (port 8085)
│   ├── feature-service/        # Features hebdomadaires (port 8086)
│   ├── graph-service/          # Construction graphes (port 8087)
│   ├── studentVle.csv          # Dataset OULAD (VLE events)
│   ├── studentInfo.csv         # Dataset OULAD (infos étudiants)
│   └── transform_oulad.py     # Script transformation CSV
│
├── ai-service/                 # Service IA Python (port 8090)
│   ├── app/
│   │   ├── main.py            # API FastAPI
│   │   ├── trainer.py         # Entraînement modèle
│   │   ├── model.py           # Modèle ML
│   │   └── schemas.py         # Schémas Pydantic
│   └── requirements.txt       # Dépendances Python
│
└── frontend/                   # Interface React (port 5173)
    ├── src/
    │   ├── components/        # Composants React
    │   ├── api.js             # Configuration API
    │   └── App.jsx            # Application principale
    └── package.json           # Dépendances Node.js
```

---

## 🔄 Flux de Données

```
1. Import CSV (Admin)
   └─> feature-service → Base de données MySQL

2. Entraînement IA (Admin)
   └─> feature-service → ai-service → Modèle sauvegardé

3. Visualisation Graphe (Teacher/Student)
   └─> graph-service → feature-service → ai-service → Prédiction Risk Score
```

---

## 🛠️ Technologies Utilisées

### Backend
- **Java 17+** avec **Spring Boot**
- **Spring Cloud Gateway** (routing)
- **Spring Security** (authentification JWT)
- **MySQL** (base de données)
- **JPA/Hibernate** (ORM)

### AI Service
- **Python 3.9+**
- **FastAPI** (API REST)
- **scikit-learn** (machine learning)
- **pandas** (traitement données)

### Frontend
- **React 19**
- **Vite** (build tool)
- **Tailwind CSS** (styling)
- **Axios** (HTTP client)
- **Cytoscape.js** (visualisation graphes)

---

## 🚀 Démarrage Rapide

### Prérequis

1. **Java 17+** installé
2. **Maven** installé
3. **MySQL** installé et démarré
4. **Python 3.9+** installé
5. **Node.js 18+** installé

### Étape 1 : Base de Données

```bash
# Démarrer MySQL
# Créer les bases de données (elles seront créées automatiquement)
# Configuration par défaut:
# - Host: localhost
# - Port: 3306
# - User: root
# - Password: (vide)
```

### Étape 2 : Backend Services

Ouvrir **7 terminaux** et démarrer chaque service dans l'ordre :

```bash
# Terminal 1 - Gateway (port 8080)
cd backend/gateway-service
./mvnw spring-boot:run

# Terminal 2 - Auth Service (port 8081)
cd backend/auth-service
./mvnw spring-boot:run

# Terminal 3 - Student Service (port 8082)
cd backend/student-service
./mvnw spring-boot:run

# Terminal 4 - Course Service (port 8083)
cd backend/course-service
./mvnw spring-boot:run

# Terminal 5 - Tracking Service (port 8084)
cd backend/tracking-service
./mvnw spring-boot:run

# Terminal 6 - Enrollment Service (port 8085)
cd backend/enrollment-service
./mvnw spring-boot:run

# Terminal 7 - Feature Service (port 8086)
cd backend/feature-service
./mvnw spring-boot:run

# Terminal 8 - Graph Service (port 8087)
cd backend/graph-service
./mvnw spring-boot:run
```

**Note Windows PowerShell :**
```powershell
# Utiliser mvnw.cmd au lieu de ./mvnw
.\mvnw.cmd spring-boot:run
```

### Étape 3 : AI Service

```bash
cd ai-service

# Créer environnement virtuel (si pas déjà fait)
python -m venv venv

# Activer l'environnement
# Windows:
venv\Scripts\activate
# Linux/Mac:
source venv/bin/activate

# Installer dépendances
pip install -r requirements.txt

# Démarrer le service (port 8090)
uvicorn app.main:app --host 127.0.0.1 --port 8090
```

### Étape 4 : Frontend

```bash
cd frontend

# Installer dépendances (si pas déjà fait)
npm install

# Démarrer le serveur de développement (port 5173)
npm run dev
```

### Étape 5 : Accéder à l'Application

Ouvrir le navigateur : **http://localhost:5173**

---

## 📊 Ports des Services

| Service | Port | Description |
|---------|------|-------------|
| Gateway | 8080 | Point d'entrée unique |
| Auth | 8081 | Authentification |
| Student | 8082 | Gestion étudiants |
| Course | 8083 | Gestion cours |
| Tracking | 8084 | Suivi activités |
| Enrollment | 8085 | Inscriptions |
| Feature | 8086 | Features hebdomadaires |
| Graph | 8087 | Construction graphes |
| AI Service | 8090 | Prédictions IA |
| Frontend | 5173 | Interface utilisateur |

---

## 🧪 Test de l'Application

### 1. Créer un Compte Admin

1. Aller sur http://localhost:5173
2. Cliquer sur "Créer un compte"
3. Remplir :
   - Email : `admin@deepedugraph.com`
   - Password : `admin123`
   - Rôle : **ADMIN**
4. Cliquer sur "S'inscrire"
5. Se connecter avec ces identifiants

### 2. Transformer le Dataset OULAD

```bash
cd backend

# Installer pandas si nécessaire
pip install pandas

# Générer weekly_features.csv
python transform_oulad.py
```

Le fichier `weekly_features.csv` sera créé dans le dossier `backend/`.

### 3. Importer le CSV

1. Dans l'interface Admin, section "Import Dataset"
2. Cliquer sur "Choisir un fichier"
3. Sélectionner `backend/weekly_features.csv`
4. Cliquer sur "Importer CSV"
5. Attendre le message de confirmation

### 4. Entraîner le Modèle IA

1. Dans l'interface Admin, section "Entraînement IA"
2. Cliquer sur "Entraîner le Modèle"
3. Attendre le message "Modèle entraîné avec succès"

### 5. Visualiser un Graphe

1. Se connecter en tant que **TEACHER** ou **ADMIN**
2. Cliquer sur "Charger le Graphe"
3. Le graphe s'affiche avec le risk score calculé

## 🔄 Workflow Complet

```
1. Démarrer tous les services
   ↓
2. Créer compte ADMIN
   ↓
3. Transformer dataset OULAD → weekly_features.csv
   ↓
4. Importer CSV via interface Admin
   ↓
5. Entraîner modèle IA
   ↓
6. Visualiser graphe étudiant (Teacher/Admin)
   ↓
7. Analyser risk score prédit
```

---

## 📚 Ressources

- **Documentation Spring Boot** : https://spring.io/projects/spring-boot
- **Documentation FastAPI** : https://fastapi.tiangolo.com
- **Documentation React** : https://react.dev
- **Dataset OULAD** : https://analyse.kmi.open.ac.uk/open_dataset

---

## ✅ Checklist de Démarrage

- [ ] MySQL démarré
- [ ] Tous les services backend démarrés (7 services)
- [ ] AI Service démarré (port 8090)
- [ ] Frontend démarré (port 5173)
- [ ] Compte ADMIN créé
- [ ] CSV transformé et importé
- [ ] Modèle IA entraîné
- [ ] Graphe visualisé avec succès

## Démonstration Video :
Lien vers la démonstration vidéo (YouTube)
https://youtu.be/QvPuAuPFRbk
