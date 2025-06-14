# 🛠️ Guide de Déploiement : Stack Monitoring & Observabilité IA

## 📦 Contenu de la stack

Cette stack open source permet de superviser et observer les métriques, logs et traces d'un modèle IA en production.

### Services inclus :
- **Prometheus** – collecte et exposition des métriques
- **Grafana** – visualisation et tableaux de bord
- **Loki** – agrégation de logs
- **OpenTelemetry Collector** – collecte et envoi de métriques
- *(+ fichiers de configuration adaptés)*

## ✅ Prérequis

- Docker & Docker Compose installés
- Ports disponibles : `3000`, `9090`, `3100`, `8889`
- Dossier local contenant les fichiers de configuration (`ai_monitoring_stack.zip` décompressé)

## 🚀 Étapes de déploiement

### 1. 📁 Cloner ou extraire le répertoire

```bash
unzip ai_monitoring_stack.zip
cd ai-monitoring-stack
```

Arborescence attendue :
```
.
├── docker-compose.yaml
├── prometheus/
│   └── prometheus.yml
├── loki/
│   └── local-config.yaml
└── otel/
    └── otel-collector-config.yaml
```

### 2. 🧱 Lancer la stack

```bash
docker-compose up -d
```

Cela démarre les 4 services :
- `grafana` sur [http://localhost:3000](http://localhost:3000)
- `prometheus` sur [http://localhost:9090](http://localhost:9090)
- `loki` sur [http://localhost:3100](http://localhost:3100)

### 3. 🔐 Connexion à Grafana

- Accéder à [http://localhost:3000](http://localhost:3000)
- Identifiants :
  - **Utilisateur** : `admin`
  - **Mot de passe** : `admin`

⚠️ Changez le mot de passe à la première connexion.

### 4. ➕ Ajouter les sources de données dans Grafana

Dans l’interface Grafana :
- Ajouter une source Prometheus : `http://prometheus:9090`
- Ajouter une source Loki : `http://loki:3100`

### 5. 📊 Créer vos premiers dashboards

- Utilisez les labels exposés par OpenTelemetry ou ajoutez des dashboards standards pour visualiser :
  - Latence, erreurs, appels IA
  - Logs applicatifs, dérives, exceptions
  - Charge CPU/RAM du modèle IA

### 6. ⚙️ Exposer vos modèles IA au monitoring

Ajoutez à votre code Python :

```python
from prometheus_client import Counter, start_http_server

inference_counter = Counter('model_inferences_total', 'Nombre d’inférences IA')
start_http_server(8000)

# Dans votre fonction d’inférence
inference_counter.inc()
```

Puis ajoutez ce port comme target dans `prometheus.yml`.

## 🔒 Bonus sécurité

- Activez HTTPS sur Grafana avec un reverse proxy (NGINX, Traefik)
- Restreignez l’accès par IP ou via authentification OIDC
- Connectez à un système de logs centralisé si besoin (SIEM, ELK)

## 📚 Ressources complémentaires

- [Grafana Loki Documentation](https://grafana.com/docs/loki/)
- [OpenTelemetry Collector](https://opentelemetry.io/docs/collector/)
- [ML Monitoring avec Prometheus](https://prometheus.io/docs/introduction/overview/)
