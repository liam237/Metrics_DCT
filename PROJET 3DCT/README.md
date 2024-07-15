
# 3DCT Project

## Description
Le projet 3DCT consiste en la création d'un exportateur de métriques de performance réseau à l'aide de Docker et Prometheus. L'exportateur est un script Python qui effectue des pings et expose les résultats pour la surveillance par Prometheus. Ce projet inclut la configuration de Docker, Docker Compose et Prometheus pour une intégration et un déploiement simples.

## Prérequis
Avant de commencer, assurez-vous d'avoir les outils suivants installés sur votre machine :
- Docker
- Docker Compose

## Installation et Lancement

### 1. Créer le fichier Dockerfile
Créez un fichier nommé `Dockerfile` dans le même répertoire que le script `ping-exporter.py` avec le contenu suivant :
```Dockerfile
# Utilisez une image de base Python
FROM python:2

# Définir le répertoire de travail
WORKDIR /opt

# Copier le script Python dans le conteneur
COPY ping-exporter.py .

# Définir les variables d'environnement
ENV PORT 80

# Exposer le port
EXPOSE 80

# Commande à exécuter
CMD ["python2", "/opt/ping-exporter.py", "-p", "80"]
```

### 2. Construire l'image Docker
Dans votre terminal, exécutez la commande suivante pour construire l'image Docker :
```bash
docker build -t ping-exporter .
```

### 3. Créer le fichier Docker Compose
Créez un fichier `docker-compose.yml` pour exécuter Prometheus et l'exportateur ensemble :
```yaml
version: '3'
services:
  exporter:
    image: ping-exporter
    environment:
      - PORT=80
    ports:
      - "8000:80"
  prometheus:
    image: prom/prometheus
    ports:
      - "9000:9090"
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml
```

### 4. Construire et démarrer les services
Utilisez la commande suivante pour construire et démarrer les différents services définis dans votre fichier `docker-compose.yml` :
```bash
docker-compose up --build
```
Accédez à l'interface de Prometheus à l'adresse suivante : [http://127.0.0.1:9000](http://127.0.0.1:9000)

### 5. Exécuter l'exportateur depuis Docker Compose
Pour démarrer l'exportateur avec Docker Compose, exécutez la commande suivante :
```bash
docker-compose up
```
Vérifiez si le conteneur est bien démarré et assurez-vous que cela fonctionne en accédant à : [http://127.0.0.1:8000/?target=1.1.1.1](http://127.0.0.1:8000/?target=1.1.1.1)

## Configuration de Prometheus
Ajoutez les configurations manquantes dans votre fichier `prometheus.yml` pour intégrer l'exportateur. Accédez à l'interface de Prometheus pour visualiser les métriques et les graphes à l'adresse suivante : [http://127.0.0.1:9000](http://127.0.0.1:9000)

---

Pour toute question ou problème, veuillez consulter la documentation technique ou contacter l'équipe de développement.

## AUTEUR
FONKUI WILLIAM
