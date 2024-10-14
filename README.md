# Docker Deployment

To start and run the application using Docker Compose:

1. Ensure Docker and Docker Compose are installed on your system.
2. Navigate to the project directory containing the `docker-compose.yml` file.
3. Run the following command:

   ```
   docker-compose up -d
   ```

   This will build and start all the containers defined in the `docker-compose.yml` file.

4. Once the containers are up and running, you can access the application through your web browser. The client site will be running on `http://localhost:3000` and the server will be running on `http://localhost:5000`.

5. Alternatively, you can directly pull the Docker images from Docker Hub and run the containers using the following commands:

   ```
   docker run -d -p 5000:5000 riot3a/yolo-backend:v1.0.0
   docker run -d -p 3000:80 riot3a/yolo-frontend:v1.0.0
   ```

This will pull the images from Docker Hub and run the containers on the specified ports.

## Docker Images

The following Docker images are built for this project:

- `riot3a/yolo-frontend:v1.0.0`
- `riot3a/yolo-backend:v1.0.0`

These images are available on Docker Hub. Here's a screenshot of the images in the Docker Hub repository:

<img width="1001" alt="Screenshot 2024-10-14 at 15 59 59" src="https://github.com/user-attachments/assets/c72b2c7e-e1e1-49b3-bfc1-8ecaa5ced192">

