# Dockerizing 3 Tier Application

This project demonstrates a complete 3-tier web application using Docker. Each component — frontend, backend, and database — runs in its own isolated container. The setup uses Docker Compose to simplify the orchestration and communication between these services.

---

## 📁 Project Structure

```
.
├── docker-compose.yaml
├── README.md
└── mern
    ├── frontend
    │   ├── Dockerfile
    │   └── ...
    └── backend
        ├── Dockerfile
        └── ...
```

---

## ⚙️ Components

### ✅ Frontend

- Built using Node.js and Vite (or another dev server).
- Runs on port `5173`.
- Served via the command: `npm run dev`

### ✅ Backend

- Node.js server application.
- Communicates with the database to serve data to the frontend.
- Runs on port `5050`.
- Starts via: `npm start`

### ✅ MongoDB (Database)

- Uses the official MongoDB Docker image.
- Data is persisted using Docker volumes.
- Accessible on port `27017`.

---

## 🐳 Docker Compose Configuration

All services are defined in the `docker-compose.yaml` file:

```yaml
services:
  frontend:
    build: mern/frontend
    ports:
      - "5173:5173"
    networks:
      - mern

  backend:
    build: mern/backend
    ports:
      - "5050:5050"
    networks:
      - mern
    depends_on:
      - mongo

  mongo:
    image: mongo
    container_name: mongodb
    ports:
      - "27017:27017"
    networks:
      - mern
    environment:
      - MONGO_INITDB_ROOT_USERNAME=admin
      - MONGO_INITDB_ROOT_PASSWORD=secret
    volumes:
      - mongo_data:/data/db

networks:
  mern:
    driver: bridge

volumes:
  mongo_data:
    driver: local
```

---

## 🏁 Getting Started

### 1. Clone the Repository

```bash
git clone https://github.com/your-username/Dockerizing-3-tier-application.git
cd Dockerizing-3-tier-application
```

### 2. Run with Docker Compose

```bash
docker compose up --build
```

> This will build and run all three services — frontend, backend, and database.

---

## 🌐 Access the Application

- Frontend: [http://localhost:5173](http://localhost:5173)
- Backend API: [http://localhost:5050](http://localhost:5050)

---



## 🗂️ Dockerfile Summary

### `frontend/Dockerfile`

```Dockerfile
FROM node:20-alpine

WORKDIR /app

COPY . .

RUN npm install

EXPOSE 5173

CMD ["npm", "run", "dev"]
```

### `backend/Dockerfile`

```Dockerfile
FROM node:20-alpine

WORKDIR /app

COPY . .

RUN npm install

EXPOSE 5050

CMD ["npm", "start"]
```

---

## 🧼 Stop and Clean Up

To stop the application:

```bash
docker compose down
```

To also remove volumes:

```bash
docker compose down -v
```

---

## 📌 Notes

- Environment variables for MongoDB are configured in the compose file.
- The frontend and backend communicate over a custom bridge network called `mern`.

---

## 📖 License

This project is licensed for educational purposes.
