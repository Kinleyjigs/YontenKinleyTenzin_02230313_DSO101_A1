# Local Development Setup (Without Docker)

## Prerequisites
- PostgreSQL installed on your Mac
- Node.js 18+

## Step 1: Check if PostgreSQL is Installed

```bash
psql --version
```

If not installed:
```bash
# Install with Homebrew
brew install postgresql@15
brew services start postgresql@15
```

## Step 2: Create Database

```bash
# Connect to PostgreSQL
psql postgres

# In psql prompt, create database and user:
CREATE DATABASE todo_db;

# Create a user (optional - or use default 'postgres' user)
CREATE USER todo_user WITH PASSWORD 'yourpassword';

# Grant privileges
GRANT ALL PRIVILEGES ON DATABASE todo_db TO todo_user;

# Connect to the database
\c todo_db

# Verify connection
\l  # List all databases

# Exit
\q
```

## Step 3: Update Backend .env File

Edit `backend/.env`:
```env
DB_HOST=localhost
DB_USER=postgres          # or todo_user if you created one
DB_PASSWORD=yourpassword  # your actual password
DB_NAME=todo_db
DB_PORT=5432
PORT=5000
```

## Step 4: Run Backend Locally

```bash
cd backend
npm install
npm start
```

The backend will automatically create the `tasks` table on startup!

## Step 5: Run Frontend Locally

```bash
# In another terminal
cd frontend
npm install
npm start
```

Frontend will run on http://localhost:3000

## Verify Database

```bash
# Connect to database
psql -U postgres -d todo_db

# Check if tasks table exists
\dt

# View tasks
SELECT * FROM tasks;

# Exit
\q
```

## Common Issues

### Port 5000 Already in Use
MacOS uses port 5000 for AirPlay. Change in `backend/.env`:
```env
PORT=5001
```

And update `frontend/.env`:
```env
REACT_APP_API_URL=http://localhost:5001
```

### Database Connection Refused
```bash
# Start PostgreSQL service
brew services start postgresql@15

# Check if it's running
brew services list
```

### Permission Denied
```bash
# Connect as superuser
psql -U postgres

# Or use your Mac username
psql -U $(whoami) postgres
```
