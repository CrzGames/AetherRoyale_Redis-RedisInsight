# AetherRoyale – Redis / RedisInsight (Kubernetes)

Déploiement de Redis pour les environnements Kubernetes du projet **Aether Royale**.

* Staging : Redis standalone (simple, léger, non HA)
* Production : Redis HA (à prévoir)
* Monitoring : RedisInsight (interface web sécurisée)

<br /><br />

---

<br /><br />

# 📦 Structure du dépôt

```
AetherRoyale_Redis-RedisInsight/
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
* Accès `kubectl` configuré
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

# 🚀 Installation Redis / RedisInsight

## 🧪 Installation – Staging

Déployer Redis :

```bash
helm upgrade --install staging-redis bitnami/redis \
  -n staging-redis \
  --create-namespace \
  -f k8s/staging/values.yaml
```

Déployer RedisInsight :

```bash
kubectl apply -f k8s/staging/redisinsight.yaml
```

## 🏭 Installation – Production

Déployer Redis :

```bash
helm upgrade --install prod-redis bitnami/redis \
  -n prod-redis \
  --create-namespace \
  -f k8s/production/values.yaml
```

Déployer RedisInsight :

```bash
kubectl apply -f k8s/production/redisinsight.yaml
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

## 🔒 Protection par BasicAuth

Créer le fichier auth :

```bash
sudo apt-get update && sudo apt-get install -y apache2-utils
htpasswd -nbB admin 'MONPASSWORD' > auth
```

Créer le secret Kubernetes :

```bash
# modifier le namespace si besoin entre staging / prod
kubectl -n staging-db create secret generic redisinsight-basic-auth --from-file=auth
```

<br /><br />

---

<br /><br />

# Accès RedisInsight

## Staging

```
https://staging.redisinsight.aetherroyale.crzgames.com/
```

## Production

```
https://redisinsight.aetherroyale.crzgames.com/
```

Un login/mot de passe BasicAuth sera demandé avant l’accès.

<br /><br />

---

<br /><br />

# 🧱 Notes Architecture

## Staging

* Redis standalone
* 1 PVC de 5Go
* Pas de haute disponibilité
* 1 seule pod
* RedisInsight exposé via Ingress sécurisé

## Production

* Réplication Redis
* Haute disponibilité
* Sauvegardes automatiques
* Monitoring avancé

<br /><br />

---

<br /><br />

# 🔒 Bonnes pratiques

* Ne jamais exposer Redis publiquement
* Accès uniquement interne au cluster
* Toujours utiliser des mots de passe forts
* Garder RedisInsight protégé par BasicAuth
* Utiliser des credentials différents entre staging et production
