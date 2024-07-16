# Note: This version allow multiple instances of the app to be deployed on the same VM.

# Get started

- Make `.env` (Make necessary changes.)

```
# You should remove this information once the initial deployment is done.

# These are no longer used.
# BACKEND_PORT=3000 # We will not expose backend port.
# POSTGRES_HOST=pf-db # This will be dynamically set.
# NGINX_PROXY=http://pf-backend:3000 # This will be dynamically set.

POSTGRES_USER=postgres
POSTGRES_PORT=5432
POSTGRES_DB=mydb
POSTGRES_APP_USER=appuser

# Change here
POSTGRES_PASSWORD=1234 #can change
POSTGRES_APP_PASSWORD=1234
PROJECT_NAME=g00 # Need to be unique
FRONTEND_PORT=5173 # Need to be unique
BACKEND_IMAGE_NAME=GITHUB_ACCOUNT/preflight-backend:latest
FRONTEND_IMAGE_NAME=GITHUB_ACCOUNT/preflight-frontend:latest
```

- `docker compose up -d --force-recreate`

# Setup database

- `docker exec -it [DB_CONTAINER_NAME] bash`
- `psql -U postgres -d mydb`
- Don't forget to change the password.

```
REVOKE CONNECT ON DATABASE mydb FROM public;
REVOKE ALL ON SCHEMA public FROM PUBLIC;
CREATE USER appuser WITH PASSWORD '1234';
CREATE SCHEMA drizzle;
GRANT ALL ON DATABASE mydb TO appuser;
GRANT ALL ON SCHEMA public TO appuser;
GRANT ALL ON SCHEMA drizzle TO appuser;
```

- `docker exec -it [BACKEND_CONTAINER_NAME] sh`
- `npm run db:generate`
- `npm run db:migrate`
