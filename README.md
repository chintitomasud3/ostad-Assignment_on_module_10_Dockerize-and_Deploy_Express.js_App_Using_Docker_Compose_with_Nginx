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
