# WAF-SHIELD

WAF-SHIELD est un projet Django de laboratoire composé de deux parties liées :

- `site_laboratoire` : le site web volontairement simple sur lequel les tests autorisés sont effectués.
- `WAF-SHIELD` : la couche de protection qui surveille, détecte, bloque, journalise, alerte et aide à l'enquête.

## Objectif

Le projet vise à protéger un site-laboratoire contre des attaques web courantes tout en permettant :

- la détection ;
- le blocage ;
- la journalisation ;
- l'alerte ;
- la supervision ;
- l'investigation après incident.

## Structure principale

```text
/waf_shield/
├── manage.py
├── requirements.txt
├── Dockerfile
├── docker-compose.yml
├── nginx.conf
├── README.md
├── configuration/
├── applications/
├── static/
├── journaux/
└── scripts/
```

## Modules applicatifs

- `applications/site_laboratoire` : pages de test autorisées, formulaire de connexion, commentaire, zone sensible simulée.
- `applications/bouclier_waf` : middleware, analyse de requêtes, détection, calcul de risque, blocage IP, journalisation.
- `applications/supervision` : tableau de bord, statistiques et alertes temps réel via WebSocket.
- `applications/enquete_numerique` : corrélation d'événements, chronologie et génération de rapport.
- `applications/simulateur_incidents` : scénarios de test limités au site-laboratoire.
- `applications/notifications` : envoi des alertes email.

## Technologies

- Python
- Django
- PostgreSQL
- Redis
- Django Channels
- Docker
- Docker Compose
- Nginx
- HTML / CSS / JavaScript léger

## Dépendances à installer

Pour exécuter le projet dans de bonnes conditions, il faut installer :

- Docker Desktop, avec Docker Compose activé ;
- un moteur Docker fonctionnel pour lancer PostgreSQL, Redis, MailHog et Nginx ;
- aucune installation manuelle de PostgreSQL, Redis ou Nginx n'est nécessaire si tu passes par Docker.

## Lancement

```bash
docker compose up --build
```

Dans un autre terminal :

```bash
docker compose run --rm web python manage.py migrate
docker compose run --rm web python manage.py createsuperuser
```

## Accès utiles

- site-laboratoire Django : `http://localhost:8000/`
- proxy Nginx : `http://localhost:8080/`
- supervision : `http://localhost:8000/supervision/`
- simulateur : `http://localhost:8000/simulateur/`
- aperçu enquête : `http://localhost:8000/enquete/apercu/`
- MailHog : `http://localhost:8025/`

## Notes d'architecture

- les noms métier sont rédigés en français ;
- les tests sont prévus uniquement sur le site-laboratoire ;
- Redis est utilisé pour la couche Channels et les alertes temps réel ;
- Daphne est utilisé dans Docker pour que l'ASGI et les WebSockets soient pris en charge proprement ;
- Nginx relaie le trafic HTTP et WebSocket vers l'application Django.
