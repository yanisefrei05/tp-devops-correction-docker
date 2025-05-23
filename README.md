# TP DevOps Correction Docker

Correction de la partie Docker du module DevOps. Amusez-vous bien avec GitHub Actions !
# TP DevOps – CI/CD avec GitHub Actions, Docker Hub et SonarCloud

Ce projet est une API Java Spring Boot configurée avec une pipeline DevOps complète intégrée à GitHub Actions.  
Elle inclut :
- l’intégration continue (CI)
- la livraison continue (CD)
- l’analyse de qualité de code (SonarCloud)

---

## 🔧 Technologies utilisées

- **Java 21**
- **Spring Boot 3.4.2**
- **Maven**
- **GitHub Actions**
- **Docker & Docker Hub**
- **SonarCloud**

---

## 🚀 Fonctionnement de la pipeline

### 1. ✅ Intégration Continue (CI)

Dès qu’un push ou une pull request est réalisé sur les branches `main` ou `develop` :
- le code est automatiquement cloné
- Java 21 est installé
- le projet est compilé avec Maven (`mvn clean verify`)
- les tests unitaires et d’intégration sont exécutés

➡️ Le job CI est défini dans `.github/workflows/main.yml`, sous le nom `test-backend`.

---

### 2. 📦 Livraison Continue (CD)

Si les tests passent :
- une image Docker est construite automatiquement pour chaque composant :
  - `simple-api/` (backend)
  - `database/` (PostgreSQL)
  - `httpd/` (reverse proxy Apache)
- chaque image est poussée sur Docker Hub (si la branche est `main` uniquement)

➡️ Ce job s'appelle `build-and-push-docker-image` et utilise `docker/build-push-action@v6`.

---

### 3. 🔍 Analyse de qualité de code avec SonarCloud

Après l'étape de tests, une analyse est déclenchée avec :
```bash
mvn sonar:sonar -Dsonar.projectKey=<projectKey> -Dsonar.organization=<organization> -Dsonar.login=${{ secrets.SONAR_TOKEN }
### Voici le Shéma global de ce tp

[ push sur GitHub ]
        ↓
[ tests Maven automatisés ]
        ↓
[ analyse de qualité SonarCloud ]
        ↓
[ build d’images Docker ]
        ↓
[ push auto sur Docker Hub ]

