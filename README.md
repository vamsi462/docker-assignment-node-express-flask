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
