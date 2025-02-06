# Nexus Docker Repository with Nginx

## **Project Overview**
This project sets up **Nexus Repository Manager** as a **private Docker registry** using **Docker Compose** and **Nginx as a reverse proxy**. It provides a secure, self-hosted container registry for managing Docker images. It enables teams to store, manage, and distribute Docker images securely, without relying on public registries like Docker Hub.

## **This project was built to**

Provide a private container registry for better control over Docker images.
Improve security by using an SSL-protected reverse proxy.
Ensure faster deployments by avoiding external dependencies.
Enable local development and CI/CD pipelines without relying on external registries

## **Objectives**
- Deploy **Nexus Repository Manager** in a containerized environment.
- Secure the registry with **Nginx reverse proxy** and **SSL encryption**.
- Enable **Docker image storage** and management via Nexus.
- Allow seamless **push and pull** of images using Docker CLI.

---
## **Prerequisites**
Ensure you have installed:
- Docker v27.5.1
- Docker Compose v2.32.4
- OpenSSL (for generating SSL certificates)
- Git (optional for version control)

---
## **Installation & Setup**

### **1️⃣ Clone the Repository**
```sh
git clone https://github.com/YOUR_USERNAME/nexus-docker-compose.git
cd nexus-docker-compose
```

### **2️⃣ Generate an SSL Certificate**
```sh
openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
-keyout certs/private.key -out certs/certificate.crt \
-subj "/C=US/ST=State/L=City/O=Company/OU=IT/CN=registry.sananetco.com"
```

### **3️⃣ Start the Services**
Run the following command to start Nexus and Nginx:
```sh
sudo docker-compose up -d
```
Check if containers are running:
```sh
sudo docker ps
```

### **4️⃣ Access the Nexus UI**
- Open your browser and visit:  
  👉 **http://localhost:8081**  
- Default login credentials:
  - **Username:** `admin`
  - **Password:** Found inside the running Nexus container:
    ```sh
    sudo docker exec nexus cat /nexus-data/admin.password
    ```

### **5️⃣ Login to the Docker Registry**
```sh
docker login registry.sananetco.com
```
If SSL issues occur, trust the self-signed certificate:
```sh
sudo mkdir -p /etc/docker/certs.d/registry.sananetco.com
sudo cp certs/certificate.crt /etc/docker/certs.d/registry.sananetco.com/ca.crt
sudo systemctl restart docker
```

### **6️⃣ Push and Pull Docker Images**
Tag an image before pushing:
```sh
docker tag nginx registry.sananetco.com/nginx-test
```
Push the image to Nexus:
```sh
docker push registry.sananetco.com/nginx-test
```
To pull the image:
```sh
docker pull registry.sananetco.com/nginx-test
```

---
## **Troubleshooting**
- **Container not running?** Check logs:
  ```sh
  sudo docker logs nexus
  sudo docker logs nginx
  ```
- **SSL certificate errors?** Ensure your system and Docker trust the self-signed certificate.
- **Unauthorized error?** Ensure Nexus authentication is configured correctly.

---
## **Contributing**
Feel free to contribute by submitting **pull requests** or opening **issues** on GitHub.

---
## **License**
This project is licensed under the **MIT License**.

---
## **Acknowledgments**
- [Sonatype Nexus Repository](https://www.sonatype.com/products/nexus-repository)
- [Docker Documentation](https://docs.docker.com/)


