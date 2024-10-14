## Explanation
This file aims to explain the thinking process and the decisions made during the development of the project.

### 1. Project Setup
The first decision was to remove the existing docker files and create new ones.

### 2. Docker Image selection
For the backend service, I relied on the more recent version of the node alpine image. Alpine images are generally small in size and I needed this to enable the final image to be small in size.
The docker file for the backend service is as follows:
```dockerfile
FROM node:22-alpine

# Create app directory
WORKDIR /backend

## Copy the package.json and package-lock.json and install the dependencies
COPY package*.json ./
RUN npm install

# Copy the rest of the application
COPY . .

# Expose the port the app runs in
EXPOSE 5000

# Serve the app
CMD ["npm", "start"]
```
The frontend image was created using 2 stages. The first stage was to build the application and the second stage was to serve the application. To serve the application, I used the `nginx` alpine image as well to ensure everything was lightweight. The docker file for the frontend service is as follows:
```dockerfile
# Stage One (Build Stage)
FROM node:16-alpine AS build-stage

# Create app directory
WORKDIR /client

## Copy the package.json and package-lock.json and install the dependencies
COPY package*.json ./
RUN npm install

# Copy the rest of the application
COPY . .

# Build the application
RUN npm run build

# Stage Two (Production Stage)
FROM nginx:alpine

# Copy the build output to replace the default nginx contents.
COPY --from=build-stage /client/build /usr/share/nginx/html

# Copy the custom nginx configuration file
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Expose the port the app runs in
EXPOSE 80

# Serve the app
CMD ["nginx", "-g", "daemon off;"]
```
To ensure the frontend would run as expected, I introduced a custom `nginx.conf` file to handle the routing of the application. The file is as follows:
```nginx
server {
    listen 80;
    server_name localhost;
    root /usr/share/nginx/html;
    index index.html;

    location / {
        try_files $uri $uri/ /index.html;
    }
}
```

### 3. Docker Compose
The next step was to create a `docker-compose.yml` file to orchestrate the services. The services included the frontend service `yolo-cleint:v1.0.0`, the baceknd service `yolo-backend:v1.0.0` and the mongodb service from the latest mongodb image `mongo:latest`.
The `docker-compose.yml` file is as follows:
```yaml
services:
  backend:
    image: yolo-backend:v1.0.0
    build:
      context: ./backend
    ports:
      - "5000:5000"
    environment:
      - MONGODB_URI=mongodb://mongodb:27017/yolo
    networks:
      - app-network
    depends_on:
      - mongodb

  client:
    image: yolo-client:v1.0.0
    build:
      context: ./client
    ports:
      - "3000:80"
    networks:
      - app-network
    depends_on:
      - backend

  mongodb:
    image: mongo:latest
    ports:
      - "27017:27017"
    volumes:
      - mongo-data:/data/db
    networks:
      - app-network

networks:
  app-network:
    driver: bridge

volumes:
  mongo-data:
```
The services all depend on one another, and the  app-network was created to ensure the services could communicate with each other.
The volumes were created to persist the data in the mongodb container. The connection string for the mongodb service was passed as an environment variable to the backend service.
