
# Artifactory Setup Using Docker

This guide outlines how to set up **JFrog Artifactory** with a PostgreSQL database in a Docker-based environment (suitable for non-production).

## 🛠️ Prerequisites

* Docker installed

---

## 📁 Setup Environment

```bash
export JFROG_HOME=$HOME/jfrog
mkdir -p $JFROG_HOME/artifactory/var/etc/
cd $JFROG_HOME/artifactory/var/etc/
touch ./system.yaml
sudo chown -R 1030:1030 $JFROG_HOME/artifactory/var
sudo chmod -R 777 $JFROG_HOME/artifactory/var
```

---

## 🐘 Start PostgreSQL (Non-Production Only)

```bash
docker run --name postgres -itd \
  -e POSTGRES_USER=artifactory \
  -e POSTGRES_PASSWORD=password \
  -e POSTGRES_DB=artifactorydb \
  -p 5432:5432 \
  library/postgres:latest
```

---

## ⚙️ Configure `system.yaml`

```yaml
shared:
  database:
    driver: org.postgresql.Driver
    type: postgresql
    url: jdbc:postgresql://host.docker.internal:5432/artifactorydb
    username: artifactory
    password: password
```

---

## 🚀 Run Artifactory

```bash
docker run --name artifactory \
  -v $JFROG_HOME/artifactory/var/:/var/opt/jfrog/artifactory \
  -d \
  -p 8081:8081 \
  -p 8082:8082 \
  releases-docker.jfrog.io/jfrog/artifactory-oss:7.104.10
```

---

## ✅ Access Artifactory

* Modern UI: [http://localhost:8082](http://localhost:8082)
* Legacy UI: [http://localhost:8081](http://localhost:8081)

Here’s your updated section with an emoji to add some clarity and friendliness:

---

## 🔐 First-Time User and Password

* **Username** – `admin`
* **Initial Password** – `password`
* **Updated Password** – `Jfrog@123`


