
## Docker

### Prerequisites
1. **Docker Desktop**: Make sure Docker Desktop is installed on your Mac. You can download it from [Docker's official site](https://www.docker.com/products/docker-desktop).

2. **Node.js and npm**: Ensure Node.js and npm are installed. You can download them from [Node.js official site](https://nodejs.org/).

### Steps to Dockerize your Node.js app

#### 1. Create a Node.js Application
If you already have a Node.js application, skip to step 2. If not, create a simple Node.js application as follows:

1. Create a new directory for your app:
    ```sh
    mkdir my-node-app
    cd my-node-app
    ```

2. Initialize a new Node.js application:
    ```sh
    npm init -y
    ```

3. Create a `server.js` file with a basic server setup:
    ```js
    // server.js
    const express = require('express');
    const app = express();
    const port = 3000;

    app.get('/', (req, res) => {
      res.send('Hello World!');
    });

    app.listen(port, () => {
      console.log(`App running at http://localhost:${port}`);
    });
    ```

4. Install Express:
    ```sh
    npm install express
    ```

#### 2. Create a Dockerfile
1. In the root of your Node.js application directory, create a file named `Dockerfile`:
    ```Dockerfile
    # Use an official Node.js runtime as a parent image
    FROM node:21.7.1  # use node --version to check the version

    # Set the working directory in the container
    WORKDIR /usr/src/app

    # Copy package.json and package-lock.json to the working directory
    COPY package*.json ./

    # Install dependencies
    RUN npm install

    # Copy the rest of the application code to the working directory
    COPY . .

    # Expose the port the app runs on
    EXPOSE 3000

    # Command to run the app
    CMD ["node", "server.js"]
    ```

#### 3. Create a .dockerignore file
1. Create a `.dockerignore` file to prevent unnecessary files from being copied to the Docker image:
    ```
    node_modules
    npm-debug.log
    ```

#### 4. Build the Docker Image
1. Open a terminal, navigate to your app directory, and build the Docker image:
    ```sh
    docker build -t my-node-app .
    ```

#### 5. Run the Docker Container
1. After building the Docker image, you can run it as a container:
    ```sh
    docker run -p 3000:3000 -d my-node-app
    ```

2. Verify that your app is running by opening a web browser and navigating to `http://localhost:3000`. You should see "Hello World!" displayed.

#### 6. Push the Docker Image to Docker Hub
If you want to upload your Docker image to Docker Hub, follow these steps:

1. Log in to Docker Hub:
    ```sh
    docker login
    ```

2. Tag your Docker image:
    ```sh
    docker tag my-node-app your-dockerhub-username/my-node-app
    ```

3. Push the image to Docker Hub:
    ```sh
    docker push your-dockerhub-username/my-node-app
    ```
