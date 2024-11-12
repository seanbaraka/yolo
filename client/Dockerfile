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

# Production stage
FROM nginx:alpine

# Copy the build output to replace the default nginx contents.
COPY --from=build-stage /client/build /usr/share/nginx/html

# Copy the custom nginx configuration file
# COPY nginx.conf /etc/nginx/conf.d/default.conf

# Expose the port the app runs in
EXPOSE 80

# Serve the app
CMD ["nginx", "-g", "daemon off;"]
