# Docker Multi-Container Application (Node.js & Flask)

A containerized full-stack web application featuring a **Node.js/Express frontend** and a **Python/Flask backend**, orchestrated using **Docker Compose** with custom bridge networking.

---

## 🚀 Project Architecture & Services
* **Frontend Service (`frontend`):** Built with Node.js and Express, listening on port `3000`. It collects user input and passes it securely to the backend via Axios.
* **Backend Service (`backend`):** Built with Python and Flask, running on port `5000`. It processes incoming POST requests and returns JSON confirmation responses.
* **Custom Bridge Network (`vamsikrishna-network`):** Isolates container traffic and enables seamless inter-container communication using service hostnames (`backend-host`).

---

## 🛠️ Docker Configuration (`docker-compose.yaml`)

```yaml
version: '3.6'

services:
  backend:
    build: backend
    hostname: backend-host
    volumes:
      - ./backend:/app
    ports:
      - "5000:5000"
    networks:
      - vamsikrishna-network

  frontend:
    build: frontend
    hostname: frontend-host
    volumes:
      - ./frontend:/app
    environment:
      - BACKEND_URL=http://backend-host:5000/api/data
    ports:
      - "3000:3000"
    depends_on:
      - backend
    networks:
      - vamsikrishna-network

networks:
  vamsikrishna-network: {}

```

---

## 📋 Step-by-Step Build & Run Guide

1. **Clone the Repository:**
```bash
git clone [https://github.com/vamsi462/docker-assignment-node-express-flask.git](https://github.com/vamsi462/docker-assignment-node-express-flask.git)
cd docker-assignment-node-express-flask

```


2. **Build and Start Containers:**
```bash
docker compose up --build

```


3. **Access the Application:**
* Open your browser and go to: `http://localhost:3000`



---

## 🔍 Troubleshooting & Key Resolutions

### **The Inter-Container Connection Issue**

* **The Problem:** Initial requests from the Node.js frontend to the Flask backend resulted in a `connection was refused` error.
* **Root Cause:** By default, Flask binds to `127.0.0.1` (localhost), which traps the server inside its own container and blocks incoming requests from other containers on the Docker network.
* **The Fix:** Updated `backend/app.py` to explicitly bind to `0.0.0.0`:
```python
if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000, debug=True)

```



---

## ☁️ Docker Hub Image Distribution

Pre-built and tested production-ready images are published and hosted on Docker Hub:

* **Frontend Image:** `vamsikrishna19/frontend:latest`
* **Backend Image:** `vamsikrishna19/backend:latest`

---

## 💡 Why Docker?

* **Eliminates Environment Drift:** Guarantees that code runs identically across local development laptops and remote cloud production servers.
* **Instant Onboarding:** New team members can launch complex multi-container architectures in minutes.
* **Service Isolation:** Multiple services run in clean, separate containers without package or port conflicts.

```

```
