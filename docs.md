# 🚀 **Deploying & Running Jellyfish Docker Services**
This guide covers:
1. **Running Locally** (for development/testing)  
2. **Deploying to Production**  
3. **Managing the Services** (starting, stopping, logging, etc.)  

---

## 1️⃣ **Running Locally (Development Mode)**
### **Prerequisites**
- Install **Docker** and **Docker Compose**  
  - [Docker Install Guide](https://docs.docker.com/get-docker/)  
  - [Docker Compose Install](https://docs.docker.com/compose/install/)  

### **Starting the Stack**
Run the following command in the same directory as your `docker-compose.yml`:
```sh
docker compose up -d
```
- The `-d` flag runs the services in **detached mode** (background).  
- This will start:
  - **Kraken Service** (`kraken-service`)
  - **Immortal Service** (`immortal`)
  - **MongoDB** (`mongo`)
  - **Redis Stack** (`redis-stack`)

### **Checking Running Containers**
```sh
docker ps
```
This will show all running services and their exposed ports.

### **Stopping the Services**
```sh
docker compose down
```
This stops all running containers.

### **Checking Logs**
```sh
docker compose logs -f
```
- `-f` follows logs in real-time.  
- To check logs of a specific service (e.g., `kraken-service`):
  ```sh
  docker compose logs -f kraken-service
  ```

### **Rebuilding After Code Changes**
If you make changes to your app's code and want to restart:
```sh
docker compose down
docker compose up --build -d
```
- The `--build` flag ensures fresh images are built.

---

## 2️⃣ **Deploying to Production**
### **1. Set Up a Production Server**
- Use **Ubuntu 22.04** or any Linux server.  
- Install Docker & Docker Compose:
  ```sh
  curl -fsSL https://get.docker.com -o get-docker.sh
  sudo sh get-docker.sh
  sudo apt-get install docker-compose-plugin
  ```

### **2. Transfer Your Project Files**
- Clone your repository:
  ```sh
  git clone https://github.com/your-org/your-repo.git
  cd your-repo
  ```

- OR upload your `docker-compose.yml` and `.env` file manually:
  ```sh
  scp docker-compose.yml user@your-server:/home/user/
  ```

### **3. Create an `.env` File (For Secrets)**
- Create a `.env` file in the same directory as `docker-compose.yml`:
  ```sh
  nano .env
  ```
- Add your environment variables:
  ```
  NODE_ENV=production
  JWT_SECRET=your-secret-key
  TRYSPEED_API_KEY=your-secret-api-key
  ```

- Modify `docker-compose.yml` to load from `.env`:
  ```yaml
  environment:
    NODE_ENV: ${NODE_ENV}
    JWT_SECRET: ${JWT_SECRET}
    TRYSPEED_API_KEY: ${TRYSPEED_API_KEY}
  ```

### **4. Start Services in Production**
```sh
docker compose up -d
```
- Services will now run **in the background**.

### **5. Configure Auto-Restart on Reboots**
Enable the **Docker service** to auto-start on system boot:
```sh
sudo systemctl enable docker
```

---

## 3️⃣ **Managing the Services in Production**
### **Checking Running Containers**
```sh
docker ps
```

### **Checking Logs**
```sh
docker compose logs -f
```

### **Restarting Services**
```sh
docker compose down
docker compose up -d
```

### **Updating to a New Version**
If you have updated your app and pushed new images:
```sh
docker compose pull  # Pull latest images
docker compose up -d --build
docker image prune -f  # Cleanup old images
```

---

## 4️⃣ **Bonus: Using Docker Swarm for Scalability**
For **high availability** and **load balancing**, deploy using **Docker Swarm**:

### **1. Initialize Swarm Mode**
```sh
docker swarm init
```

### **2. Deploy the Stack**
```sh
docker stack deploy -c docker-compose.yml my-app
```
- This creates a **Swarm stack** and ensures it **recovers automatically**.

---

# ✅ **Summary**
| Task | Command |
|------|---------|
| **Start locally** | `docker compose up -d` |
| **Stop services** | `docker compose down` |
| **Check running containers** | `docker ps` |
| **View logs** | `docker compose logs -f` |
| **Restart services** | `docker compose down && docker compose up -d` |
| **Update production** | `docker compose pull && docker compose up -d --build` |
| **Enable auto-restart** | `sudo systemctl enable docker` |

Now you're ready to **run and deploy your app locally & in production**! 🚀