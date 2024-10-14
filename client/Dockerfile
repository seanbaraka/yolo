FROM node:22-alpine AS build-stage

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
FROM node:22-alpine AS production-stage

# Create app directory
WORKDIR /app

# Copy the build from the build-stage
COPY --from=build-stage /client/build ./build
COPY --from=build-stage /client/package*.json ./
COPY --from=build-stage /client/public ./public
COPY --from=build-stage /client/src ./src

ENV NODE_ENV=production

EXPOSE 3000

CMD ["npm", "start"]
