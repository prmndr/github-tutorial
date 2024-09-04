# Creating Your First Docker Container: Hello Docker Webpage

Welcome to this beginner-friendly guide on creating your first Docker container! By the end of this tutorial, you'll have a simple webpage displaying "Hello Docker" running in a container.

## Prerequisites
- Docker Desktop installed on your computer

## Step 1: Create a project directory

1. Open your computer's terminal or command prompt.
2. Create a new directory for your project:
   ```
   mkdir hello-docker
   cd hello-docker
   ```

## Step 2: Create a simple HTML file

1. Using a text editor, create a new file named `index.html` in the `hello-docker` directory.
2. Add the following content to `index.html`:
   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <title>Hello Docker</title>
   </head>
   <body>
       <h1>Hello Docker!</h1>
       <p>This is my first Docker container.</p>
   </body>
   </html>
   ```
3. Save the file.

## Step 3: Create a Dockerfile

1. In the same directory, create a new file named `Dockerfile` (no file extension).
2. Add the following content to `Dockerfile`:
   ```dockerfile
   FROM nginx:alpine
   COPY index.html /usr/share/nginx/html/index.html
   ```
3. Save the file.

## Step 4: Build the Docker image

1. In your terminal, make sure you're in the `hello-docker` directory.
2. Run the following command to build your Docker image:
   ```
   docker build -t hello-docker-image .
   ```
   This command builds an image and tags it as "hello-docker-image".

## Step 5: Run the Docker container

1. Now, let's run a container using our new image:
   ```
   docker run -d -p 8080:80 --name hello-docker-container hello-docker-image
   ```
   This command:
   - Runs the container in detached mode (`-d`)
   - Maps port 8080 on your computer to port 80 in the container (`-p 8080:80`)
   - Names the container "hello-docker-container"
   - Uses the "hello-docker-image" we just created

## Step 6: View your webpage

1. Open a web browser and go to `http://localhost:8080`
2. You should see your "Hello Docker" webpage!

## Step 7: Stop and remove the container

When you're done, you can stop and remove the container:

1. Stop the container:
   ```
   docker stop hello-docker-container
   ```
2. Remove the container:
   ```
   docker rm hello-docker-container
   ```

Congratulations! You've just created and run your first Docker container with a simple webpage.

