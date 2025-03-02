### **Nginx Configuration**
## **📖 Usage Guide: Running Nginx for This Setup**

### **🔹 Prerequisites**
1. **Install Docker & Docker Compose**
   - Ensure Docker and Docker Compose are installed:
     ```sh
     docker --version
     docker-compose --version
     ```
   - If missing, install them:
     - **Ubuntu/Debian:**
       ```sh
       sudo apt update && sudo apt install docker.io docker-compose -y
       ```
     - **Mac (via Homebrew):**
       ```sh
       brew install docker docker-compose
       ```

---

### **🔹 Running Locally**
1. **Clone or create the required directory**
   ```sh
   mkdir nginx-setup && cd nginx-setup
   ```
2. **Create Nginx Configuration**
   - Save the provided Nginx config as `nginx.conf` inside the directory.
   - Copy and change the `example.com` in `jellyfish_land.conf` to your domain.
   - Copy `jellyfish_land.conf` to the `conf.d` directory and import it into `nginx.conf` or `conf.d/default.conf` to recognize the new config.

3. **Create a `docker-compose.yml` File**
   ```yaml
   version: "3.8"

   services:
     nginx:
       image: nginx:latest
       container_name: nginx_proxy
       ports:
         - "443:443"
       volumes:
         - ./nginx.conf:/etc/nginx/nginx.conf:ro
         - ./conf.d:/etc/nginx/conf.d:ro
         - ./certs:/etc/ssl/certs:ro
         - ./private:/etc/ssl/private:ro
       restart: always
   ```

4. **Run Docker Compose**
   ```sh
   docker-compose up -d
   ```

---

### **🔹 Running in Production**
1. **Set up a server (Ubuntu recommended)**
   ```sh
   sudo apt update && sudo apt install -y docker.io docker-compose
   ```

2. **Ensure correct permissions**
   ```sh
   sudo usermod -aG docker $USER
   ```

3. **Upload `.env`, `nginx.conf`, `jellyfish_land.conf`, and `docker-compose.yml` to the server.**

4. **Run services**
   ```sh
   docker-compose up -d
   ```

5. **Verify containers**
   ```sh
   docker ps
   ```

---

### **🔹 Troubleshooting**
| Issue                     | Solution |
|---------------------------|----------|
| Nginx doesn't start | Run `docker logs nginx_proxy` |
| Services not reachable | Check `docker ps` and ensure they are running |
| SSL issues | Ensure certs are correct under `/etc/ssl/certs/` and `/etc/ssl/private/` |

---

**You're all set! 🚀** Now, your Nginx proxy is routing traffic to `Kraken` and `Immortal`, handling WebSocket upgrades, and ensuring secure TLS communication. 🎯