# Todo Application - DSO Assignment 1

**Student Name:** Yonten Kinley Tenzin  
**Student ID:** 02230313

---

## Overview

Full-stack todo application deployed on Render with React frontend, Express backend, and PostgreSQL database. Application is containerized with Docker and automatically deployed via GitHub integration.

---

## Active Deployment Links

**Frontend:** [https://todo-frontend-latest-zfjg.onrender.com/](https://todo-frontend-latest-zfjg.onrender.com/)

**Backend API:** [https://todo-backend-latest-kr1t.onrender.com](https://todo-backend-latest-kr1t.onrender.com)

---

## Tech Stack

| Component | Technology |
|-----------|------------|
| Frontend | React 18, Axios, CSS3 |
| Backend | Node.js 18, Express.js |
| Database | PostgreSQL |
| Containerization | Docker |
| Deployment | Render.com |
| CI/CD | GitHub + render.yaml Blueprint |

---

## Part A: Docker Hub & Render Deployment

### 1. Build Docker Images & Push to Docker Hub

```bash

docker login

# Backend
docker buildx build --platform linux/amd64 -t yonten1234567890/todo-backend:02230313 --push ./backend

# Frontend
docker buildx build --platform linux/amd64 -t yonten1234567890/todo-frontend:02230313 --push ./frontend
```

### 2. Deploy on Render

**Backend Service:**
- Image: `yonten1234567890/todo-backend`
- Port: 5000
- Environment: 
  - `DATABASE_URL` (Render PostgreSQL connection string)
  - `DB_NAME`, `DB_USER`, `DB_PASSWORD`, `DB_PORT`

**Frontend Service:**
- Image: `yonten1234567890/todo-frontend`
- Port: 80
- Environment:
  - `REACT_APP_API_URL=https://todo-backend-latest-kr1t.onrender.com`

**Database:**
- Render PostgreSQL managed service

---

## Part B: Automated Deployment

### render.yaml Blueprint

```yaml
services:
  - type: web
    name: be-todo
    env: docker
    dockerfilePath: ./backend/Dockerfile
    envVars:
      - key: DATABASE_URL
        value: ${{ secrets.DATABASE_URL }}
      - key: PORT
        value: 5000

  - type: web
    name: fe-todo
    env: docker
    dockerfilePath: ./frontend/Dockerfile
    envVars:
      - key: REACT_APP_API_URL
        value: https://todo-backend-latest-kr1t.onrender.com

databases:
  - name: todo-db
    databaseName: todo_db
    user: todo_user
```

### Deployment Process

1. Push code to GitHub
2. Connect GitHub repo to Render
3. Render auto-detects `render.yaml`
4. Services build and deploy automatically
5. Every git push triggers a new deployment

---

## Features

- Add, edit, delete tasks  
- Mark tasks as complete/incomplete  
- Real-time task updates  
- Persistent database storage  
- Responsive UI design  
- Docker containerized  
- Auto-deploy on git push  

---

## API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/` | Health check |
| GET | `/tasks` | Get all tasks |
| POST | `/tasks` | Create task |
| PUT | `/tasks/:id` | Update task |
| DELETE | `/tasks/:id` | Delete task |

---

## Environment Variables

**Backend (.env):**
```env
DATABASE_URL=postgres://user:password@host:5432/todo_db
DB_HOST=your-render-host
DB_USER=todo_user
DB_PASSWORD=your_password
DB_NAME=todo_db
DB_PORT=5432
PORT=5000
```

**Frontend (.env):**
```env
REACT_APP_API_URL=https://todo-backend-latest-kr1t.onrender.com
```

⚠️ **Never commit .env files to Git!**

---

## Deployment Screenshots

### Local Development
![Local Development Setup](frontend/public/local_dev.png)

### Docker Hub
![Docker Hub Repository](frontend/public/dockerhub.png)

### Docker Hub frontend
![alt text](frontend/public/frontend.png)

### Docker Hub backend
![alt text](frontend/public/backend.png)

### Render Deployment
![Render Dashboard](frontend/public/render.png)

### Live Application
![alt text](frontend/public/applicationUI.png)

---

## Testing

Test live backend:
```bash
curl https://todo-backend-latest-kr1t.onrender.com/tasks
```

Or visit the [frontend](https://todo-frontend-latest-zfjg.onrender.com/) directly to test the app.

---

## Key Lessons

1. **Environment variables** separate config from code
2. **Docker** ensures consistent environments across machines
3. **Render Blueprint** automates multi-service deployments
4. **GitHub integration** enables CI/CD pipelines
5. **ARM compatibility** needed for M1/M2 Macs (use buildx)
6. **DATABASE_URL** simplifies cloud database connections

---

## Resources

- [Docker Docs](https://docs.docker.com/)
- [Render Docs](https://render.com/docs)
- [Express.js](https://expressjs.com/)
- [React](https://react.dev/)
- [PostgreSQL](https://www.postgresql.org/docs/)

---


