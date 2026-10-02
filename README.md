# Classification d'images satellites
Cas pratique sur un CNN

## Cas pratique
C'est un travail individuel qui est attendu de vous. Chaque apprenant devra présenter un rapport personnel, du code personnel et un dashboard personnel. Lisez bien toutes les consignes avant de commencer.

## Compétences visées
- C.20 : Surveiller une application d’intelligence artificielle
- C.21 : Résoudre les incidents techniques

Ces deux compétences sont validées par l'épreuve E5.

## Les tickets d'incident
Chaque branche représente un ticket d'incident. Il y a 3 branches, donc 3 tickets.

Les tickets sont répartis de la façon suivante : 
- ticket 1 : 
- ticket 2 : 
- ticket 3 :
- ticket 4 : 

Vous trouverez toutes les informations du ticket dans le Readme de la branche.

## Installation
Merci de ne rien modifier sur ce dépôt.

1. Faire un fork ou un clone de la branche qui vous intéresse sur votre dépôt GitHub. Créez une branche pour la correction.
2. Installer Git en local. Récupérer le projet sur votre machine.
3. Le dossier `tests` contient des images satellite.

## Démarrage avec Docker

Depuis la racine du projet, vérifier les valeurs du fichier `.env`, puis lancer :

```powershell
docker compose up --build -d
```
| Service | Adresse par défaut |
| --- | --- |
| Client Streamlit | http://localhost:8501 |
| Documentation API | http://localhost:8081/docs |
| État API et base | http://localhost:8081/health |
| Adminer | http://localhost:8080 |
| MySQL | localhost:3306 |

Dans Adminer, utiliser le serveur `db` et les valeurs `MYSQL_USER`,
`MYSQL_PASSWORD` et `MYSQL_DATABASE` du `.env`.

```powershell
docker compose logs -f
docker compose down
```

Les données MySQL sont conservées dans `databases/`, et les images reçues dans
`api/satelite_images/`. `data/db/init.sql` est exécuté uniquement lors de la
création d'une base sur un dossier de données vide. Modifier les identifiants
dans `.env` ne modifie pas les comptes d'une base déjà initialisée.

## Configuration

Compose lit automatiquement le `.env` à la racine pour les identifiants et les
ports publiés. Il transmet à chaque application uniquement les variables dont
elle a besoin. Les connexions entre conteneurs utilisent `db:3306` et `web:80`,
indépendamment des ports publiés sur la machine.

Les configurations Python chargent ce même `.env` avec `python-dotenv` pour un
lancement local, sans écraser les variables déjà présentes dans l'environnement.
Les identifiants MySQL n'ont aucune valeur par défaut dans le code Python.
`DB_HOST`, `DB_PORT` et `API_BASE_URL` servent aux connexions locales ; Compose
les remplace par les adresses internes appropriées.

Si `API_PORT` change, adapter aussi `API_BASE_URL` pour le client lancé localement.
`UPLOAD_FOLDER` est relatif au dossier `api/` en local ; Compose fixe son chemin
au dossier persistant monté dans le conteneur.

L'API attend que MySQL soit prêt. Après modification des dépendances, relancer `docker compose up --build -d`.

## Lancement Python local

Avec Python 3.11 et les dépendances installées dans un environnement virtuel,
lancer MySQL et Adminer avec `docker compose up -d db adminer`, puis :

```powershell
cd api
python -m pip install -r requirements.txt
python -m uvicorn app.main:app --host 127.0.0.1 --port 8081
```

Dans un deuxième terminal depuis la racine :

```powershell
cd client
python -m pip install -r requirements.txt
python -m streamlit run app.py
```

Adapter le port Uvicorn si `API_PORT` a été modifié.
