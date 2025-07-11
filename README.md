# 🧠 Wiki.js Docker Installation Guide

This repository provides a quick setup for [Wiki.js](https://wiki.js.org) using Docker Compose.

---

## 📦 Prerequisites

- Docker & Docker Compose installed
- Sufficient system resources
- Optional: Public domain if using HTTPS

---

## 📁 Files

- `docker-compose.yml`: Main configuration for Docker services
- `README.md`: This guide

---

## 🚀 Installation Steps

1. Clone this repo or copy the `docker-compose.yml` file.
2. Run the following command to start Wiki.js and PostgreSQL:

```bash
docker compose up -d
```

3. Access the Wiki.js setup wizard at `http://localhost`.

---

## 🔧 docker-compose.yml

```
version: "3"
services:

  db:
    image: postgres:17-alpine
    environment:
      POSTGRES_DB: wiki
      POSTGRES_USER: wikijs
      POSTGRES_PASSWORD: wikijsrocks
    logging:
      driver: "none"
    restart: unless-stopped
    volumes:
      - db-data:/var/lib/postgresql/data

  wiki:
    image: ghcr.io/requarks/wiki:2
    depends_on:
      - db
    environment:
      DB_TYPE: postgres
      DB_HOST: db
      DB_PORT: 5432
      DB_USER: wikijs
      DB_PASS: wikijsrocks
      DB_NAME: wiki
    restart: unless-stopped
    ports:
      - "80:3000"

volumes:
  db-data:
```

---

## 🔐 Enable HTTPS (Optional)

Update the `wiki` service in the compose file:

```yaml
    environment:
      ...
      SSL_ACTIVE: "true"
      LETSENCRYPT_DOMAIN: "wiki.example.com"
      LETSENCRYPT_EMAIL: "[email protected]"
    ports:
      - "80:3000"
      - "443:3443"
```

---

## 🧼 Clean Up

To stop and remove containers:

```bash
docker compose down
```

---

## 📚 More Info

Official Docs: [https://docs.requarks.io/install/docker](https://docs.requarks.io/install/docker)