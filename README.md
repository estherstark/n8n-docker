# Local n8n Setup with Docker

This guide will help you set up [n8n](https://n8n.io) locally using Docker and Docker Compose.

## Prerequisites

- [Docker Desktop](https://www.docker.com/products/docker-desktop) (WSL2 must be enabled during installation)
- [Visual Studio Code](https://code.visualstudio.com/) (recommended)

---

## Setup Instructions

### 1. Install Docker Desktop

Download and install [Docker Desktop](https://www.docker.com/products/docker-desktop).  
Ensure **WSL2** is enabled during the installation process.

---

### 2. Create Project Folder

Open a terminal and run the following commands:

```bash
mkdir n8n
cd n8n
code .
```

### 3. Create Docker Compose File

Inside your n8n folder, create a file named docker-compose.yml with the following content:

```yaml
version: "3"

services:
  n8n:
    image: n8nio/n8n
    restart: always
    ports:
      - "5678:5678"
    env_file:
      - .env
    volumes:
      - ./n8n_data:/home/node/.n8n
```

### 4. Create .env File

Create a file named .env with the following content (or copy from a .env.example file):

```env
# Basic auth to secure your n8n instance
N8N_BASIC_AUTH_ACTIVE=true
N8N_BASIC_AUTH_USER=your_username
N8N_BASIC_AUTH_PASSWORD=your_password

# Timezone
TZ=Asia/Bangkok

# Host and port
N8N_HOST=localhost
N8N_PORT=5678
```

### 5. Create .gitignore file

create .gitignore for prevent any unwanted file up to git, content:

```bash
/.env
/n8n_data
```

### 6. Start n8n

In terminal, Start n8n service by run:

```bash
docker-compose up -d
```

### 6. Accessing n8n
Once the container is running, open your browser and go to http://localhost:5678

### 7. Set up owner account as the page show
Set up account and use n8n on your docker.


# 🧩 Optional part

## **Option A: Use Cloudflare tunnel**

When you run **n8n in Docker on your local Windows PC**, and try to **add Google OAuth credentials**, it will fail.

This is because **Google requires a public URL**, but your n8n is only running at something like `http://localhost:5678`, which **Google can't access**.

To fix this, you need to **set up a Cloudflare Tunnel**, which gives your local n8n a **secure public URL** that Google can reach.


## **Option B: Deploy to AWS**

If you're done testing and ready to go live, then spin up an AWS instance.

### **✅ Steps:**

1. Set up an EC2 instance (Ubuntu recommended)
2. Clone Github project: https://github.com/estherstark/n8n-docker.git
2. Install Docker
3. Assign a **domain name** (e.g. n8n.yourdomain.com)
4. Set up **HTTPS** using Nginx + Certbot or Traefik
5. Configure your **Google OAuth** credentials with:

    ```bash
    https://n8n.yourdomain.com/rest/oauth2-credential/callback
    ```
    
Note: If yourdomain is not set to static one, you should set Elastic IP on AWS to prevent address changing every time you restart the EC2 instance.