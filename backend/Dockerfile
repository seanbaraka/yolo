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
