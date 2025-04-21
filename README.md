# WIK-DPS-TP03


## Objectif

Ce projet met en place un environnement Docker Compose comprenant :
- Un service API (basé sur Node.js + TypeScript)
- 4 réplicas de l'API pour le load balancing
- Un reverse-proxy NGINX exposé sur le port 8080

Le reverse proxy distribue les requêtes entrantes de manière équilibrée sur les différents réplicas de l'API.

---

## Structure du projet

```
WIK-DPS-TP03/
├── docker-compose.yml
├── nginx.conf
├── Dockerfile.multi
├── server.ts
├── package.json
├── package-lock.json
├── tsconfig.json
└── README.md
```

## Démarrage du projet

### 1. Cloner le projet :

```
git clone https://github.com/HB313/WIK-DPS-TP03.git && cd WIK-DPS-TP03
```

### 2. Lancer l'environnement :

```
docker-compose up --build
```

### 3. Tester l'API : Aller sur : http://localhost:8080/ping

Chaque appel au endpoint /ping renverra le hostname du conteneur ayant traité la requête.

Exemple de réponse :

```
{
  "hostname": "39e979a7adcb",
  "headers": { ... }
}
```

## Fonctionnement technique

- **API Node.js / Express** : expose une route `/ping` qui logge le `hostname` du conteneur.

- **Dockerfile.multi** : multi-stage build pour créer une image optimisée.

- **Docker Compose** :
  - 4 instances de l'API déployées automatiquement.
  - Reverse proxy NGINX configuré pour loadbalancer entre les 4 réplicas.

- **NGINX** :
  - Écoute sur le port 80 à l'intérieur du conteneur.
  - Exposé sur le port 8080 de l'hôte.
  - Load balance toutes les requêtes `/ping` vers les API disponibles.
