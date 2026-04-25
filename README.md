#  DevOps Project: Automated Web Deployment

##  Overview

This project demonstrates a full CI/CD pipeline for deploying a web application to a VPS using:

- Docker (containerization)
- Nginx (reverse proxy)
- Ansible (infrastructure provisioning)
- GitHub Actions (CI/CD)

The system is designed to automatically deploy updates on every push to the main branch.

---

## Architecture

Local Machine
      ↓
GitHub (push)
      ↓ 
GitHub Actions (CI/CD)
      ↓ 
VPS (Docker container)
      ↓ 
Nginx (reverse proxy)
      ↓ 
User (via HTTP/HTTPS) 

---

## Tech Stack

- Docker
- Nginx
- Ansible
- GitHub Actions
- Linux (Ubuntu VPS)

---

## Project Structure

project/
  ├── ansible/
  │    ├── inventory.ini
  │    ├── playbook.yml
  │    └── roles/ 
  │         ├── docker/
  │         └── nginx/ 
  ├── app/
  │    └── index.html
  ├── docker/
  │    └── Dockerfile
  ├── nginx/ 
  │    └── mysite.conf
  ├── .github/ 
  │    └── workflows/ 
  │         └── deploy.yml 
  │ 
  └── README.md 

---

## How It Works

### 1. Infrastructure Setup (Ansible)

Ansible is used to configure the server:

- installs Docker
- installs and configures Nginx
- sets up reverse proxy
- prepares the server for deployment

Run manually:

ansible-playbook -i ansible/inventory.ini ansible/playbook.yml 

---

### 2. Application Containerization

The application is packaged into a Docker container:

 FROM nginx:alpine 
 COPY app /usr/share/nginx/html 

---

### 3. CI/CD Pipeline (GitHub Actions)

On every push to main:

1. Build Docker image
2. Save image as archive
3. Copy to VPS via SCP
4. Deploy via SSH:
   - load image
   - stop old container
   - run new container

---

## Deployment Flow

git push → GitHub Actions → build → transfer → deploy → live update 

---

## Nginx Configuration

Nginx acts as a reverse proxy:

server {
     listen 80;     
     server_name avrusanov.ru www.avrusanov.ru;
     location / {
         proxy_pass http://127.0.0.1:8080;    
     }
} 

---

## Verification

After deployment:

curl http://127.0.0.1:8080 
curl http://localhost 

Or open in browser:

http://avrusanov.ru

---

## Known Limitations

- Uses .tar image transfer instead of Docker registry
- No zero-downtime deployment
- No monitoring/alerting

---

## Future Improvements

- Use Docker Registry (Docker Hub / GHCR)
- Implement zero-downtime deployment
- Add HTTPS automation via Ansible
- Add monitoring (Prometheus + Grafana)

---

## Notes

This project focuses on demonstrating practical DevOps fundamentals:

- Infrastructure as Code
- CI/CD automation
- Container-based deployment
- Reverse proxy configuration

---

## Author

Avrusanov
