# WEHR - Application de Gestion RH

## 📋 Contexte du Test Technique

Ce projet a été développé dans le cadre du **test technique pour le poste de Développeur Full-Stack chez QuanTech**.

### Énoncé du Test

L'objectif était de réaliser une application web de gestion des ressources humaines en se basant sur les maquettes Figma fournies, en démontrant les compétences suivantes :

- ✅ Reproduction fidèle des interfaces (Desktop, Tablette, Mobile)
- ✅ Conception et développement d'APIs RESTful
- ✅ Implémentation d'opérations CRUD complètes
- ✅ Application des bonnes pratiques (design patterns, architecture clean)
- ✅ Intégration d'aspects de sécurité (validation, protection)
- ✅ Gestion appropriée de l'état de l'application
- ✅ Documentation du code et justification des choix techniques

**Lien Figma** : [HR Dashboard Responsive](https://www.figma.com/design/ORTCfe98P9FUPJ6UBIAmHM/HR-Dashboard-Responsive--Community-?node-id=149-462&p=f&t=tqeuAzNgJZbi9FGe-0)

---

## 🚀 Démarrage Rapide

### Prérequis

- **Docker** et **Docker Compose** installés sur votre machine
- Ports disponibles : `80`, `3000`, `5432`, `8080`

### Installation en Une Commande

```bash
# Cloner le projet avec les submodules
git clone --recurse-submodules git@github.com:AlMoustapha01/quantech-wehr.git
cd quantech-wehr

# OU si vous avez déjà cloné sans --recurse-submodules
git clone git@github.com:AlMoustapha01/quantech-wehr.git
cd quantech-wehr
git submodule update --init --recursive

# Démarrer tous les services
docker compose up -d
```

**C'est tout !** L'application se configure automatiquement sans aucune configuration manuelle additionnelle.

> **Note** : Ce projet utilise des **git submodules** pour gérer le frontend (`we-hr`) et le backend (`we-hr-api`) comme des dépôts séparés. Assurez-vous d'utiliser `--recurse-submodules` lors du clone ou d'exécuter `git submodule update --init --recursive` après le clone.

### Accès à l'Application

Une fois les conteneurs démarrés, vous pouvez accéder aux services suivants :

| Service                  | URL                                   | Description                       |
| ------------------------ | ------------------------------------- | --------------------------------- |
| **🎨 Frontend**          | http://wehr.localhost                 | Interface utilisateur principale  |
| **🔌 Backend API**       | http://api.wehr.localhost             | API RESTful                       |
| **📚 Documentation API** | http://api.wehr.localhost/api/v1/docs | Documentation Swagger interactive |
| **🔧 Traefik Dashboard** | http://localhost:8080                 | Monitoring du reverse proxy       |

### Commandes Utiles

```bash
# Arrêter tous les services
docker compose down

# Voir les logs en temps réel
docker compose logs -f

# Voir les logs d'un service spécifique
docker compose logs -f wehr_backend
docker compose logs -f wehr_frontend

# Redémarrer les services
docker compose restart

# Reconstruire les images
docker compose up -d --build
```

---

## 📖 Description du Projet

**WEHR** (We HR) est une application complète de gestion des ressources humaines comprenant :

### Fonctionnalités Implémentées

#### 🎯 Dashboard

- Vue d'ensemble des métriques RH (employés actifs, départements, recrutements)
- Statistiques en temps réel
- Graphiques et visualisations

#### 👥 Gestion des Employés

- Liste complète avec recherche et filtres
- Création, modification et suppression d'employés
- Affichage des détails (profil, département, statut)
- Validation complète des données

#### 🏢 Gestion des Départements

- CRUD complet des départements
- Association avec les employés
- Statistiques par département
- Gestion des managers

#### 🔗 Affectations

- Attribution employé ↔ département
- Historique des affectations
- Validation des contraintes métier

#### 🎯 Recrutement

- Gestion des offres d'emploi
- Suivi des candidatures
- Statuts de recrutement

#### 📅 Planification

- Calendrier des événements RH
- Gestion des horaires

### Design Responsive

L'application est **entièrement responsive** et optimisée pour :

- 🖥️ **Desktop** (prioritaire) - 1920px et plus
- 💻 **Tablette** - 768px à 1919px
- 📱 **Mobile** - 320px à 767px

---

## 🏗️ Architecture Technique

### Vue d'Ensemble

```
┌─────────────────┐
│   Traefik       │  Reverse Proxy (Port 80)
│   (Port 80)     │  Routage automatique
└────────┬────────┘
         │
    ┌────┴────────────────┐
    │                     │
┌───▼──────┐      ┌───────▼─────┐
│ Frontend │      │   Backend   │
│ Next.js  │◄─────┤   NestJS    │
│ (3000)   │ API  │   (3000)    │
└──────────┘      └───────┬─────┘
                          │
                   ┌──────▼──────┐
                   │  PostgreSQL │
                   │   (5432)    │
                   └─────────────┘
```

### Stack Technique

#### Frontend (`we-hr/`)

- **Framework** : Next.js 16 (App Router) + React 19
- **Langage** : TypeScript 5
- **Styling** : Tailwind CSS 4 + shadcn/ui
- **State Management** : Zustand + React Query (TanStack Query)
- **Forms** : React Hook Form + Zod
- **UI Components** : Radix UI
- **Icons** : Lucide React
- **Animations** : Motion (Framer Motion)

#### Backend (`we-hr-api/`)

- **Framework** : NestJS 11
- **Langage** : TypeScript 5
- **Base de données** : PostgreSQL 16
- **ORM** : TypeORM 0.3
- **Documentation** : Swagger/OpenAPI
- **Validation** : class-validator + Zod
- **Sécurité** : Helmet, CORS, CSP

#### Infrastructure

- **Containerisation** : Docker + Docker Compose
- **Reverse Proxy** : Traefik v3
- **Base de données** : PostgreSQL 16 Alpine

---

## 🎨 Choix Architecturaux

### 1. Architecture Clean (Backend)

Le backend suit les principes de **Clean Architecture** avec :

- **Domain Layer** : Entités métier et logique business
- **Use Cases** : Cas d'usage applicatifs
- **Infrastructure** : Implémentation technique (DB, API)
- **Presentation** : Controllers et DTOs

**Avantages** :

- Séparation claire des responsabilités
- Testabilité maximale
- Indépendance vis-à-vis des frameworks
- Maintenabilité à long terme

### 2. Feature-Based Architecture (Frontend)

Le frontend est organisé par **fonctionnalités métier** :

```
src/
├── features/
│   ├── employees/      # Tout ce qui concerne les employés
│   ├── departments/    # Tout ce qui concerne les départements
│   ├── recruitment/    # Gestion du recrutement
│   └── schedule/       # Planification
├── components/         # Composants UI réutilisables
└── lib/               # Utilitaires partagés
```

**Avantages** :

- Scalabilité facilitée
- Code organisé par domaine métier
- Réutilisabilité des composants UI
- Collaboration en équipe simplifiée

### 3. Sécurité Multi-Couches

#### Frontend

- **Content Security Policy (CSP)** stricte
- Validation des formulaires avec Zod
- Protection XSS et Clickjacking

#### Backend

- **Helmet** : Headers de sécurité HTTP
- **CORS** : Whitelist d'origines autorisées
- **Validation** : class-validator sur tous les DTOs
- **HSTS** : Force HTTPS en production
- Protection contre les injections SQL (TypeORM)

### 4. Gestion d'État Optimisée

- **Server State** : React Query (cache, synchronisation API)
- **Client State** : Zustand (état global léger)
- **Form State** : React Hook Form (performances optimales)

---

## 📁 Structure du Projet

```
QUANTECH/
├── we-hr/                     # Frontend Next.js
│   ├── src/
│   │   ├── app/              # App Router (pages)
│   │   ├── features/         # Modules métier
│   │   ├── components/       # Composants UI
│   │   └── lib/              # Utilitaires
│   ├── Dockerfile
│   └── package.json
│
├── we-hr-api/                # Backend NestJS
│   ├── src/
│   │   ├── app/
│   │   │   ├── domain/       # Entités métier
│   │   │   ├── use-cases/    # Logique applicative
│   │   │   ├── infrastructure/
│   │   │   └── presentation/ # Controllers
│   │   ├── config/
│   │   └── database/
│   ├── Dockerfile
│   └── package.json
│
├── docker-compose.yaml       # Orchestration
├── .env                      # Configuration
└── README.md                # Ce fichier
```

---

## 🔧 Configuration

### Variables d'Environnement

Le fichier `.env` à la racine contient toutes les configurations :

```env
# Application
APP_PORT=3000
APP_NAME="WEHR"
APP_API_PREFIX="api/v1"
NODE_ENV=production

# Base de données
POSTGRES_USER=wehr_user
POSTGRES_PASSWORD=wehr_password
POSTGRES_DB=wehr_db
DATABASE_HOST=wehr_database
DATABASE_PORT=5432

# CORS (origines autorisées)
APP_CORS_ORIGIN=http://wehr.localhost,http://localhost:3000,http://localhost:3001
```

**Aucune modification n'est nécessaire** pour le démarrage initial.

---

## 🔒 Sécurité Implémentée

### Frontend

- ✅ Content Security Policy (CSP) stricte
- ✅ Protection XSS via headers
- ✅ Protection Clickjacking (`frame-ancestors 'none'`)
- ✅ Validation des formulaires (Zod)
- ✅ HTTPS forcé en production

### Backend

- ✅ Helmet (headers de sécurité)
- ✅ CORS configuré avec whitelist
- ✅ Validation automatique des DTOs
- ✅ Protection injection SQL (TypeORM)
- ✅ HSTS avec preload
- ✅ X-Content-Type-Options: nosniff
- ✅ X-XSS-Protection activé

### Note sur l'Authentification

⚠️ **L'authentification JWT n'a pas été implémentée par manque de temps.**  
L'infrastructure est prête (Passport, JWT dans les dépendances), mais les endpoints d'authentification et les guards ne sont pas encore actifs. Cela reste une priorité pour une version future.

---

## 📚 Documentation Détaillée

Chaque sous-projet possède sa propre documentation :

- **Frontend** : [`we-hr/README.md`](./we-hr/README.md)
  - Stack technique détaillée
  - Architecture des composants
  - Guide de développement
- **Backend** : [`we-hr-api/README.md`](./we-hr-api/README.md)
  - Architecture Clean détaillée
  - Documentation des endpoints
  - Patterns utilisés

---

## 🧪 Tests

### Backend

```bash
cd we-hr-api
pnpm install
pnpm test              # Tests unitaires
pnpm test:e2e          # Tests end-to-end
pnpm test:cov          # Couverture de code
```

### Frontend

```bash
cd we-hr
pnpm install
pnpm test              # Tests Vitest
pnpm lint              # ESLint
```

---

## 🎯 Réalisations du Test

### ✅ Objectifs Atteints

| Critère                   | Statut | Détails                                               |
| ------------------------- | ------ | ----------------------------------------------------- |
| **Interfaces responsive** | ✅     | Desktop, Tablette, Mobile                             |
| **APIs RESTful**          | ✅     | CRUD complet pour Employees, Departments, Assignments |
| **Opérations CRUD**       | ✅     | Toutes les entités principales                        |
| **Bonnes pratiques**      | ✅     | Clean Architecture, DDD, Feature-based                |
| **Sécurité**              | ⚠️     | Validation, Headers, CORS (JWT non implémenté)        |
| **Gestion d'état**        | ✅     | React Query + Zustand                                 |
| **Documentation**         | ✅     | README complets, code commenté                        |
| **Docker**                | ✅     | `docker compose up -d` fonctionnel                    |

### 🚧 Améliorations Futures

- [ ] Authentification JWT complète (guards, refresh tokens)
- [ ] Tests E2E avec Playwright
- [ ] Pagination côté serveur
- [ ] Upload de photos de profil
- [ ] Export de données (CSV, PDF)
- [ ] Notifications temps réel (WebSockets)
- [ ] Gestion des permissions (RBAC)
- [ ] Internationalisation (i18n)

---

## 💡 Points Forts du Projet

### 1. **Déploiement Zero-Config**

Une seule commande suffit : `docker compose up -d`

### 2. **Architecture Scalable**

- Clean Architecture backend
- Feature-based frontend
- Séparation claire des responsabilités

### 3. **Developer Experience**

- TypeScript strict partout
- Hot reload en développement
- Documentation Swagger interactive
- Storybook pour les composants

### 4. **Performance**

- React Server Components
- React Compiler activé
- Optimisations Next.js automatiques
- Cache intelligent avec React Query

### 5. **Sécurité**

- Multi-couches (frontend + backend)
- Headers de sécurité configurés
- Validation stricte des données

---

## 👤 Auteur

**Almoustapha Doumbia**  
Développeur Full-Stack  
Test technique - QuanTech

---

## 📄 Licence

MIT
