# public-ifs-taxe-soft-project
Données publiques du projet

# Documentation de l'API Flask pour la gestion des tickets

Cette API permet de gérer des tickets en utilisant une base de données PostgreSQL. Elle est sécurisée par une clé API et offre des fonctionnalités pour ajouter, récupérer et vérifier des tickets.

---

## **Table des matières**
1. [Prérequis](#prérequis)
2. [Installation](#installation)
3. [Configuration](#configuration)
4. [Routes de l'API](#routes-de-lapi)
   - [Ajouter un ticket](#ajouter-un-ticket)
   - [Récupérer tous les tickets](#récupérer-tous-les-tickets)
   - [Récupérer les tickets par intervalle de dates](#récupérer-les-tickets-par-intervalle-de-dates)
   - [Vérifier un ticket](#vérifier-un-ticket)
   - [Récupérer le dernier ticket](#récupérer-le-dernier-ticket)
5. [Exemples d'utilisation](#exemples-dutilisation)
6. [Sécurité](#sécurité)
7. [Déploiement](#déploiement)

---

## **Prérequis**

- Python 3.9 ou supérieur
- PostgreSQL
- Bibliothèques Python : `Flask`, `psycopg2`

---

## **Installation**

1. Clonez le dépôt :
   ```bash
   git clone https://github.com/votre_utilisateur/votre_repo.git
   cd votre_repo
   ```

2. Créez un environnement virtuel :
   ```bash
   python -m venv .venv
   source .venv/bin/activate  # Sur macOS/Linux
   .venv\Scripts\activate     # Sur Windows
   ```

3. Installez les dépendances :
   ```bash
   pip install -r requirements.txt
   ```

---

## **Configuration**

1. **Variables d'environnement :**
   Créez un fichier `.env` à la racine du projet avec les variables suivantes :
   ```plaintext
   DB_HOST=votre_host
   DB_PORT=votre_port
   DB_NAME=votre_nom_de_base
   DB_USER=votre_utilisateur
   DB_PASSWORD=votre_mot_de_passe
   DB_CONNECTION_STRING=postgresql://user:password@host:port/dbname
   API_KEY=votre_cle_api_secrete
   ```

2. **Lancer l'API :**
   ```bash
   flask run
   ```

---

## **Routes de l'API**

### **Ajouter un ticket**
- **URL :** `/ajouter_ticket`
- **Méthodes :** `GET`, `POST`
- **Authentification :** Clé API requise dans les en-têtes (`X-API-KEY`) ou les paramètres de requête (`api_key`).
- **Paramètres :**
  - `ticket_code` : Code du ticket (obligatoire).
  - `montant` : Montant du ticket (obligatoire).
- **Exemple de requête `POST` :**
  ```bash
  curl -X POST http://127.0.0.1:5000/ajouter_ticket \
       -H "Content-Type: application/json" \
       -H "X-API-KEY: votre_cle_api_secrete" \
       -d '{"ticket_code": "ABC123", "montant": 100.50}'
  ```
- **Exemple de requête `GET` :**
  ```
  http://127.0.0.1:5000/ajouter_ticket?api_key=votre_cle_api_secrete&ticket_code=ABC123&montant=100.50
  ```
- **Réponse :**
  ```json
  {
      "message": "Ticket ajouté avec succès.",
      "ticket": {
          "id": 1,
          "ticket_code": "ABC123",
          "montant": 100.50,
          "date": "2023-10-05 12:34:56"
      }
  }
  ```

---

### **Récupérer tous les tickets**
- **URL :** `/recuperer_tickets`
- **Méthodes :** `GET`
- **Authentification :** Clé API requise dans les en-têtes (`X-API-KEY`) ou les paramètres de requête (`api_key`).
- **Paramètres optionnels :**
  - `date_debut` : Date de début au format `YYYY-MM-DD`.
  - `date_fin` : Date de fin au format `YYYY-MM-DD`.
- **Exemple de requête :**
  ```bash
  curl -X GET http://127.0.0.1:5000/recuperer_tickets \
       -H "X-API-KEY: votre_cle_api_secrete"
  ```
- **Réponse :**
  ```json
  [
      {
          "id": 1,
          "ticket_code": "ABC123",
          "montant": 100.50,
          "date": "2023-10-05 12:34:56"
      },
      {
          "id": 2,
          "ticket_code": "XYZ789",
          "montant": 200.75,
          "date": "2023-10-04 10:20:30"
      }
  ]
  ```

---

### **Récupérer les tickets par intervalle de dates**
- **URL :** `/recuperer_tickets?date_debut=YYYY-MM-DD&date_fin=YYYY-MM-DD`
- **Méthodes :** `GET`
- **Authentification :** Clé API requise dans les en-têtes (`X-API-KEY`) ou les paramètres de requête (`api_key`).
- **Exemple de requête :**
  ```bash
  curl -X GET "http://127.0.0.1:5000/recuperer_tickets?api_key=votre_cle_api_secrete&date_debut=2023-10-01&date_fin=2023-10-31"
  ```
- **Réponse :**
  ```json
  [
      {
          "id": 1,
          "ticket_code": "ABC123",
          "montant": 100.50,
          "date": "2023-10-05 12:34:56"
      }
  ]
  ```

---

### **Vérifier un ticket**
- **URL :** `/verifier_ticket`
- **Méthodes :** `GET`
- **Authentification :** Clé API requise dans les en-têtes (`X-API-KEY`) ou les paramètres de requête (`api_key`).
- **Paramètres :**
  - `ticket_code` : Code du ticket à vérifier (obligatoire).
- **Exemple de requête :**
  ```bash
  curl -X GET "http://127.0.0.1:5000/verifier_ticket?api_key=votre_cle_api_secrete&ticket_code=ABC123"
  ```
- **Réponse :**
  ```json
  {
      "message": "Ticket trouvé.",
      "ticket": {
          "id": 1,
          "ticket_code": "ABC123",
          "montant": 100.50,
          "date": "2023-10-05 12:34:56"
      }
  }
  ```

---

### **Récupérer le dernier ticket**
- **URL :** `/get_last_ticket`
- **Méthodes :** `GET`, `POST`
- **Authentification :** Clé API requise dans les en-têtes (`X-API-KEY`) ou les paramètres de requête (`api_key`).
- **Exemple de requête `GET` :**
  ```bash
  curl -X GET http://127.0.0.1:5000/get_last_ticket \
       -H "X-API-KEY: votre_cle_api_secrete"
  ```
- **Exemple de requête `POST` :**
  ```bash
  curl -X POST http://127.0.0.1:5000/get_last_ticket \
       -H "X-API-KEY: votre_cle_api_secrete"
  ```
- **Réponse :**
  ```json
  {
      "message": "Dernier ticket trouvé.",
      "ticket": {
          "id": 1,
          "ticket_code": "ABC123",
          "montant": 100.50,
          "date": "2023-10-05 12:34:56"
      }
  }
  ```

---

## **Exemples d'utilisation**

### **Ajouter un ticket**
```bash
curl -X POST http://127.0.0.1:5000/ajouter_ticket \
     -H "Content-Type: application/json" \
     -H "X-API-KEY: votre_cle_api_secrete" \
     -d '{"ticket_code": "ABC123", "montant": 100.50}'
```

### **Récupérer tous les tickets**
```bash
curl -X GET http://127.0.0.1:5000/recuperer_tickets \
     -H "X-API-KEY: votre_cle_api_secrete"
```

### **Vérifier un ticket**
```bash
curl -X GET "http://127.0.0.1:5000/verifier_ticket?api_key=votre_cle_api_secrete&ticket_code=ABC123"
```

### **Récupérer le dernier ticket**
```bash
curl -X GET http://127.0.0.1:5000/get_last_ticket \
     -H "X-API-KEY: votre_cle_api_secrete"
```

---

## **Sécurité**

- **Clé API :** Toutes les routes nécessitent une clé API valide dans les en-têtes (`X-API-KEY`) ou les paramètres de requête (`api_key`).
- **HTTPS :** Utilisez HTTPS pour chiffrer les données en transit.
- **Limitation des accès :** Configurez des règles de pare-feu pour limiter l'accès à l'API.

---

## **Déploiement**

1. **Sur Render pour le test :**
   - Liez votre dépôt GitHub à Render.
   - Configurez les variables d'environnement (`DB_HOST`, `DB_PORT`, `DB_NAME`, `DB_USER`, `DB_PASSWORD`, `DB_CONNECTION_STRING`, `API_KEY`).
   - Déployez l'application.

2. **Sur un serveur :**
   - Installez PostgreSQL et configurez la base de données.
   - Utilisez un serveur WSGI comme `gunicorn` pour déployer l'API.

---

## **Contribuer**

Les contributions sont les bienvenues ! Pour contribuer :
1. Forkez le projet.
2. Créez une branche (`git checkout -b feature/nouvelle-fonctionnalité`).
3. Committez vos changements (`git commit -m 'Ajouter une nouvelle fonctionnalité'`).
4. Poussez la branche (`git push origin feature/nouvelle-fonctionnalité`).
5. Ouvrez une Pull Request.

---

## **Licence**

Ce projet est sous licence MIT. Voir le fichier [LICENSE](LICENSE) pour plus de détails.

---

Cette documentation est conçue pour être claire et complète, permettant aux utilisateurs de comprendre et d'utiliser facilement votre API.
