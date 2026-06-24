# ostad-Assignment_on_module_10_Dockerize-and_Deploy_Express.js_App_Using_Docker_Compose_with_Nginx

# 1. Clone the Repository
```
git clone https://github.com/roy35-909/Module-3-deployment.git
cd Module-3-deployment
```

# Dockerize the Express.js App (Create/Update Dockerfile) (Already created on cloned repo)
```
# Use official Node.js image (alpine for smaller size)
FROM node:18-alpine

# Set working directory
WORKDIR /app

# Copy package files first (better caching for npm install)
COPY package*.json ./

# Install dependencies (production only)
RUN npm ci --only=production

# Copy the rest of the application
COPY . .

# Expose the port the app runs on (from package.json / server.js)
EXPOSE 5000

# Environment variable for port
ENV PORT=5000

# Start the app
CMD ["npm", "start"]
```
# 2  Create a docker-compose.yml 
##  nginx Configuration File 
```mkdir -p nginx
```
### Create file nginx/nginx.conf
```
events {
    worker_connections 1024;
}

http {
    upstream node_backend {
        server app:5000;
    }

    server {
        listen 80;
        server_name localhost;

        location / {
            proxy_pass http://node_backend;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }
    }
}
```

## Create docker-compose.yml
```
version: '3.8'

services:
  nginx:
    image: nginx:alpine
    ports:
      - "8080:80"
    volumes:
      - ./nginx/nginx.conf:/etc/nginx/nginx.conf:ro
    depends_on:
      - app
    restart: unless-stopped

  app:
    image: chintitomasud/module3-express-app:latest
    # build: .         
    restart: unless-stopped
```
## Docker Compose run
```
sudo docker compose up -d --build
```

