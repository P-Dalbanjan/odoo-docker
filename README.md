# Odoo 17 Docker Deployment

This repository provides a **minimal Docker Compose setup** for running **Odoo 17 with PostgreSQL 15** using Docker secrets and persistent volumes.

---

# 🏗️ Architecture Overview

The stack contains two services orchestrated using Docker Compose:

**Odoo Web Application**

* Runs **Odoo 17**
* Exposes the web interface on **port 8069**
* Supports custom addons
* Uses persistent storage for Odoo data

**PostgreSQL Database**

* Runs **PostgreSQL 15**
* Stores all Odoo databases
* Uses persistent storage for database files
* Password managed via **Docker Secrets**

---

# 🚀 Quick Start

## 1. Prerequisites

Make sure the following are installed on your system:

* Docker
* Docker Compose

Installation guides:

* [https://docs.docker.com/engine/install/](https://docs.docker.com/engine/install/)
* [https://docs.docker.com/compose/install/](https://docs.docker.com/compose/install/)

---

## 2. Setup Database Password

Create the PostgreSQL password file used by Docker secrets.

```bash
echo "your-secure-password" > odoo_pg_pass
```

This file will be mounted securely inside containers as a secret.

**Important:**
Do **not commit this file to version control**.

---

## 3. Start the Stack

Run the containers in detached mode:

```bash
docker compose up -d
```

Docker will automatically:

* Pull required images
* Create volumes
* Start Odoo and PostgreSQL
* Connect both services

---

# 🌐 Access Odoo

After deployment, open your browser and visit:

```
http://localhost:8069
```

Or on a remote server:

```
http://your-server-ip:8069
```

---

# 📂 Project Structure

Example structure for this setup:

```
project-root
│
├── docker-compose.yml
├── odoo_pg_pass
│
├── config/
│   └── odoo.conf
│
└── addons/
    └── custom_modules
```

---

# 🛠️ Configuration Details

## Docker Secrets

The PostgreSQL password is managed using **Docker Secrets**.

```
odoo_pg_pass
```

This file is mounted inside containers as:

```
/run/secrets/postgresql_password
```

Both **Odoo** and **PostgreSQL** read the password securely from this location.

---

## Persistent Volumes

To ensure data persists across container restarts, the following volumes are used:

| Volume          | Purpose                                |
| --------------- | -------------------------------------- |
| `odoo-web-data` | Stores Odoo filestore and session data |
| `odoo-db-data`  | Stores PostgreSQL database files       |

These volumes are automatically created by Docker.

---

## Custom Addons

Custom modules can be added to:

```
./addons
```

This directory is mounted inside the container at:

```
/mnt/extra-addons
```

Odoo will automatically detect modules from this location.

---

## Odoo Configuration

Custom configuration files can be placed in:

```
./config
```

Mounted to:

```
/etc/odoo
```

Typical file:

```
config/odoo.conf
```

---

# 🛑 Stopping the Stack

To stop the services:

```bash
docker compose down
```

To stop **and remove volumes**:

```bash
docker compose down -v
```

⚠️ This will delete all database data.

---

# 📦 Docker Images Used

* **Odoo:** `odoo:17.0`
* **PostgreSQL:** `postgres:15`

---

## ⚖️ License

This project is released under the **Unlicense**, making it free and unencumbered software released into the public domain. You are free to copy, modify, publish, or sell this software for any purpose.

For more information, please refer to the [LICENSE](https://github.com/dalbanjan-git/odoo-docker?tab=Unlicense-1-ov-file) file or [unlicense.org](https://unlicense.org).
