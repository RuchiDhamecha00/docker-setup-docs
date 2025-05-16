# Docker Setup Documentation

This README provides a comprehensive overview of Docker, its system requirements, installation steps, and how to set up a backend using Docker, Docker Compose, and related services like MongoDB and PostgreSQL.

---

## Table of Contents
1. [What is Docker?](#what-is-docker)
2. [Docker Image](#docker-image)
3. [Docker Container](#docker-container)
4. [System Requirements for Docker Desktop](#system-requirements-for-installing-docker-desktop)
5. [Step-by-Step Installation Guide](#step-by-step-installation-guide)
   1. [Download Docker Desktop](#1-download-docker-desktop)
   2. [Install Docker Desktop](#2-install-docker-desktop)
6. [Setting Up Backend in Docker Desktop](#setting-up-backend-in-docker-desktop)
   1. [Obtain the Backend Code](#step-1-obtain-the-backend-code)
   2. [Add Required Docker Files](#step-2-add-required-docker-files)
   3. [Build and Run the Containers](#step-3-build-and-run-the-containers)
7. [Configuring MongoDB Replication in Docker](#configuring-mongodb-replication-in-docker)
8. [Adding a Database in PostgreSQL (pgAdmin)](#adding-a-database-in-postgresql-pgadmin)
9. [Import JSON Files into MongoDB Using MongoDB Compass](#import-json-files-into-mongodb-using-mongodb-compass)
10. [Docker - Common Commands](#docker---common-commands)

---

## What is Docker?

- **Virtualization Software**  
- **Makes developing and deploying applications much easier**  
- **Packages the application with all necessary dependencies, configuration, system tools, and runtime**  

Docker allows you to build, ship, and run applications quickly and reliably across different computing environments.


### How Docker Works

Docker uses a **client-server architecture**, where:

- **Docker Client**: A command-line interface that communicates with the Docker Daemon.  
- **Docker Daemon**: Manages images, containers, networks, and volumes on the host.  
- **Docker Images**: Serve as templates to create Docker Containers.  
- **Docker Containers**: Running instances of Docker Images.

You can manage Docker containers using:
- **Docker CLI** (Command-Line Interface)  
- **Docker Desktop** (Graphical User Interface)

---

## Docker Image

A **Docker Image** is a lightweight, standalone, and executable software package that includes:

- Application code
- Runtime environment
- System libraries and dependencies
- Configuration files

**Images** are read-only templates that serve as the blueprint for creating Docker Containers. They can be stored in container registries like [Docker Hub](https://hub.docker.com/) or private registries.

- **Command to view images**:
  ```bash
  docker images
## Docker Container

A **Docker Container** is a runtime instance of a Docker Image. It is a lightweight, isolated, and portable environment that runs applications consistently across different systems.
Each container runs as an independent process on the host machine, sharing the OS kernel but maintaining separate execution environments.
- **Command to list running containers in docker desktop terminal**  
  ```bash
  docker ps
## **System Requirements for Installing Docker Desktop**

| **Category**    | **Windows**                                         | **macOS**                                        | **Linux**                                      |
|----------------|-----------------------------------------------------|-------------------------------------------------|------------------------------------------------|
| **OS Version** | Windows 10 Pro, Enterprise, or Education (1909+), Windows 11 (except Home) | macOS Ventura (13), Monterey (12), or Big Sur (11) | Ubuntu 22.04/20.04, Debian 11+, Fedora 37+, Arch Linux |
| **Processor**  | 64-bit CPU with SLAT support                        | Apple Silicon (M1, M2, or later) or Intel (2010+) | 64-bit CPU                                     |
| **RAM**        | Minimum: 4 GB <br> Recommended: 8 GB+               | Minimum: 4 GB <br> Recommended: 8 GB+          | Minimum: 4 GB <br> Recommended: 8 GB+         |
| **Storage**    | At least 4 GB of free disk space                    | At least 4 GB of free disk space               | At least 4 GB of free disk space              |
| **Virtualization** | Enabled in BIOS/UEFI <br> Hyper-V & WSL 2 required | Not required                                   | Required for GUI-based Docker Desktop         |
| **Kernel Version** | N/A                                             | N/A                                             | Minimum: Kernel 5.15+                         |
| **Other Requirements** | Administrative privileges, Internet access  | Administrative privileges, Internet access     | Root/sudo access, Internet access            |


## Step-by-Step Installation Guide

### 1. Download Docker Desktop
Visit the official Docker website: [Docker Get Started](https://www.docker.com/get-started/). Choose the appropriate version and download the installer.

### 2. Install Docker Desktop
#### **Windows**
1. Run the installer (`Docker Desktop Installer.exe`).
2. Click **Install** and wait for the process to complete.
3. Open Docker Desktop and follow the setup wizard.

#### **macOS**
1. Open the downloaded `.dmg` file.
2. Drag `Docker.app` to the Applications folder.
3. Launch Docker from Applications.
4. Follow the setup wizard and grant necessary permissions.

#### **Linux**
```sh
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
sudo usermod -aG docker $USER
sudo systemctl enable --now docker
docker --version
```

## Setting Up Backend in Docker Desktop

### 1. Obtain the Backend Code
#### **Cloning from GitHub**
```sh
git clone <backend-repository-url>
cd <backend-folder>
```
#### **Receiving a ZIP File**
Extract it into a dedicated folder.

### 2. Add Required Docker Files
Ensure you have the following Docker files:

✅ `Dockerfile` – Defines the main backend service  
✅ `docker-compose.yml` – Defines the multi-container application setup  

## Configuration Validation for Docker-Compose Services

Ensure the following configuration files contain the correct values as per the `docker-compose.yml` file.

### 1. Celery Configuration
- **File:** `backend/server/celery_config/development.py`
- **Broker URL:** Ensure the RabbitMQ service name and host are correctly mentioned.

  **Example:**
  ```python
  broker_url = "pyamqp://keywordio:keywordio2022@adhelp-rabbitmq:5672/adhelp_vhost"

- **File:**  `/server/config/settings/development.py` 

  - Ensure the Redis service name and host are correctly set.
      ```python
         CHANNEL_LAYERS = {
         "default": {
         "BACKEND": "channels_redis.core.RedisChannelLayer",
         "CONFIG": {
               "hosts": [("adhelp-redis", 6379)],
            },
         },
      }

  
  - Ensure the PostgreSQL service HOST and NAME (db_name) is correctly set.
      ```
      DATABASES = {

      'default': {

        'ENGINE': 'django.db.backends.postgresql_psycopg2',

        'NAME': 'new_live_db',

        'USER': 'postgres',

        'PASSWORD': 'keywordio2022',

        'HOST': 'adhelp-postgres',

        'PORT': '5432',

        }

    }
    ```
  - Ensure the MongoDB service name and host are correctly set in the DB_DOMAIN.
      ```
      MONGO_DATABASE = {
         "CONNECTION_SERVER": "mongodb://",
         "DB_DOMAIN": "adhelp-mongo:27017",
         "NAME": "devFeedDatabase",
         "USERNAME": "keywordio",
         "PASSWORD": "keywordio2022",
         "AUTH_MACHANISM": "SCRAM-SHA-1",
         "AUTH_SOURCE_DB": "admin",
         "TIMEOUT": 90000
         }
      ```
    

### 3. Build and Run the Containers
Run following commands in terminal to build containers.
```sh
docker-compose build
docker-compose up -d
docker ps
```
![image](https://github.com/user-attachments/assets/0e6232ed-be2e-4415-b8a3-d2a35de4a013)

Ensure services like **web, postgres, mongo, redis, rabbitmq, celery, celery-beat** are running.


## Adding a Database in PostgreSQL (pgAdmin)
1. Open pgAdmin in your browser: [http://localhost:5050/login?next=/](http://localhost:5050/login?next=/)
2. Login with default credentials:
   - **Email**: `admin@admin.com`
   - **Password**: `admin`
3. Add a New Server:
   - **General Tab** → Name: `Postgres`
   - **Connection Tab**:
     - Host: `postgres`
     - Port: `5432`
     - Username: `postgres` {your postgres service name}
     - Password: `keywordio2022`
   - Click **Save**
   ![image](https://github.com/user-attachments/assets/552a1b9d-ada2-42c0-a505-fc0bcd49dd41)

4. Create a Database:
   - Navigate to **Servers > Postgres > Databases**
   - Right-click **Databases** → **Create > Database**
   - Name the database: `<database_name_mentioned_in_dockercompose>`
   - Click **Save**
![image](https://github.com/user-attachments/assets/6ec60a3e-a444-464e-bf03-abff6da27bb6)


## Import JSON Files into MongoDB Using MongoDB Compass
1. Open **MongoDB Compass**
2. Connect to:
   ```sh
   mongodb://keywordio:keywordio2022@localhost:27017/?authSource=admin
   ```
3. Click **Connect**
4. Create a Database:
   - **Database Name**: `devFeedDatabase`
   - Click **Create**
5. Import JSON Files:
   - Open your database
   - Select your collection
   - Click **Import Data**
   - Choose your JSON file and click **Import**
![image](https://github.com/user-attachments/assets/38351187-277e-4c2b-8c88-2f66e30f3afa)

![image](https://github.com/user-attachments/assets/e490cbab-5311-4eaf-b5b1-7af24107085c)

### Add user in docker mongoDB container
   - Docker desktop > mongodb container > exec
      (Type ```mongosh``` in exec & start executing below commands)
    ![image](https://github.com/user-attachments/assets/152e0d0f-ed4d-47d3-9dca-eadb9698d2ad)



---

🎉 **Your Docker setup is now complete!** 

## Docker - Common Commands

### 1. Build Docker Images

```sh
docker compose build
```
- This command builds images for all services defined in docker-compose.yml

```  
docker compose build <service-name>
 ```
 - This builds the image only for a specific service.


### 2. Start Containers

```
docker compose up
```
- Starts containers using the images built.

- If images are not built, it will build them first.
```
docker compose up -d
```
- Runs containers in detached mode (in the background, so your terminal is free). [recommended]

### 3. Stop and Remove Containers

```
docker compose down
```
- Stops and removes all containers defined in the docker-compose.yml.

- Does not remove volumes (data remains intact).
```
docker compose down -v
```
- Stops and removes all containers and volumes.

- Warning: This deletes all data stored in volumes, such as database files.

### 4. Restart container
```
docker restart <container_name>
```
- Simply stops and starts the same container again without changing anything.

- Use this when you just need to refresh a service (e.g., restart PostgreSQL after a crash).

```
docker restart $(docker ps -q)
```
- to restart all running containers

