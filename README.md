# AetherRoyale – Redis / RedisInsight (Kubernetes)

Déploiement de Redis pour les environnements Kubernetes du projet **Aether Royale**.

* Staging : Redis standalone (simple, léger, non ha)
* Production : Redis HA
* Monitoring : RedisInsight (interface web)

<br /><br />

---

<br /><br />

# 📦 Structure du dépôt

```
AetherRoyale_Redis/
  k8s/
    staging/
      values.yaml
      redisinsight.yaml
    production/
      values.yaml
      redisinsight.yaml
```

Chaque dossier représente un environnement indépendant.

<br /><br />

---

<br /><br />

# 🧰 Prérequis

* Cluster Kubernetes fonctionnel
* Helm installé
* Accès kubectl configuré
* cert-manager installé (pour TLS)
* Ingress Controller NGINX installé

<br /><br />

---

<br /><br />

# ⚙️ Configuration

Éditer le fichier correspondant à l’environnement :

```
k8s/staging/values.yaml
```

ou

```
k8s/production/values.yaml
```

Remplacer :

```yaml
auth:
  enabled: true
  password: "REPLACE_ME"
```

par un mot de passe Redis fort.

<br /><br />

---

<br /><br />

# 🚀 Installation

## Ajouter le repo Helm Bitnami

```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
```

## 🧪 Installation – Staging

```bash
helm upgrade --install staging-redis bitnami/redis \
  -n staging-redis \
  --create-namespace \
  -f k8s/staging/values.yaml
```

## 🏭 Installation – Production

```bash
helm upgrade --install prod-redis bitnami/redis \
  -n prod-redis \
  --create-namespace \
  -f k8s/production/values.yaml
```

<br /><br />

---

<br /><br />

# 📡 Accès Redis depuis le cluster

## Staging

Host :

```
staging-redis-master.staging-redis.svc.cluster.local
```

Port :

```
6379
```

## Production

Host :

```
prod-redis-master.prod-redis.svc.cluster.local
```

Port :

```
6379
```

<br /><br />

---

<br /><br />

# 🔐 Variables d’environnement (backend)

## Staging

```
REDIS_HOST=staging-redis-master.staging-redis.svc.cluster.local
REDIS_PORT=6379
REDIS_PASSWORD=REPLACE_ME
```

## Production

```
REDIS_HOST=prod-redis-master.prod-redis.svc.cluster.local
REDIS_PORT=6379
REDIS_PASSWORD=REPLACE_ME
```

<br /><br />

---

<br /><br />

# 🧠 RedisInsight (Interface Web)

RedisInsight permet de :

* Visualiser les clés
* Voir la RAM utilisée
* Tester des commandes
* Debug le cache

## Installation – Staging

```bash
kubectl apply -f k8s/staging/redisinsight.yaml
```

## Installation – Production

```bash
kubectl apply -f k8s/production/redisinsight.yaml
```

<br /><br />

---

<br /><br />

## Accès

RedisInsight est accessible directement via l’Ingress privé :

```
https://redisinsight.staging.aetherroyale.crzgames.com/
```

Il suffit d’ouvrir cette URL dans le navigateur pour consulter l’interface.

<br /><br />

---

<br /><br />

# 🧱 Notes Architecture

### Staging

* Redis standalone
* 1 PVC de 5Go
* Pas de haute dispo
* 1 seule pod

### Production

TODO
