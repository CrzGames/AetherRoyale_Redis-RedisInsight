# AetherRoyale – Redis (Kubernetes)

## Staging
Déploiement d’un **Redis standalone** pour l’environnement **staging** du projet Aether Royale.

* Mode : **standalone** (pas de HA)
* Stockage : **PVC 5 Gi**

## Production


<br />

## 📦 Structure du dépôt

```
AetherRoyale_Redis/
  k8s/
    staging/
      values.yaml
```

<br />

## 🧰 Prérequis

* Un cluster Kubernetes fonctionnel
* Helm installé
* Accès `kubectl` configuré sur le cluster

<br /><br />

---

<br /><br />

## ⚙️ Configuration

Éditer :

```
k8s/staging/values.yaml
```

Remplacer :

```
REPLACE_ME
```

par un mot de passe Redis fort :

```yaml
auth:
  enabled: true
  password: "REPLACE_ME"
```

<br /><br />

---

<br /><br />

## 🚀 Installation (staging)

Ajouter le repo Helm Bitnami :

```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
```

Déployer Redis en créant automatiquement le namespace :

```bash
helm upgrade --install staging-redis bitnami/redis \
  -n staging-redis \
  --create-namespace \
  -f k8s/staging/values.yaml
```

<br /><br />

---

<br /><br />

## 📡 Accès Redis depuis le cluster

Host interne Kubernetes :

```
staging-redis-master.staging-redis.svc.cluster.local
```

Port :

```
6379
```

<br /><br />

---

<br /><br />

## 🔐 Variables d’environnement côté backend

Exemple :

```
REDIS_HOST=staging-redis-master.staging-redis.svc.cluster.local
REDIS_PORT=6379
REDIS_PASSWORD=REPLACE_ME
```
