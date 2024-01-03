<h1 align="center">React Vite + TypeScript + Docker Project ✨️</h1> 
<p align="center">This project is a template for a React application using Vite, TypeScript, and Docker.</p>

# Prerequisites

- [Node.js](https://nodejs.org/) installed
- [Docker](https://www.docker.com/) installed



# Installation Steps

### Clone the repository ⎘
```bash
git clone https://github.com/pooja-sahani-simform/react-vite-typescript-with-docker-config.git

```

### Change the working directory 
```bash
cd react-vite-with-docker
```

### Install dependency 
```bash
npm i
```
### Build the Docker Container
Run this command to build the image on your local machine and start the container. You only need to run this command the first time, and whenever you make changes to docker-compose.yml.

```bash
docker-compose up --build --no-recreate -d

```

From the second time, we can use
```bash
docker-compose up -d
```

Now our container is up and you should be able to test it using the following command.
```bash
docker-compose ps
```

### Build and start the Application
Just to clarify, we have a running container, but not the installed or running react app. For that, we need to log into the container and then execute the below commands to start the react vite application from docker container.

```bash
docker exec -it vite_docker sh -c 'npm i && npm run dev'
```

You are all set! Open [localhost:8000](http://localhost:8000/) to see the app.



# Additional Detail for customized configuration

### Update Vite Config 
We need to specify the host and port in vite.config.js in order to work with the Docker. you custimized based on your requirements.

```bash
export default defineConfig({
  plugins: [react()],
  server: {
    host: '0.0.0.0',
    port: 8000,
    watch: {
      usePolling: true,
    }
  },
})
```

### Setup Docker
I am intending to use docker-composer for easier to scale as compared to relying on Dockerfile.
First, we will need to add docker-compose.yml within the root of the project.(Note: Just verify the ports, working_dir, volumes target based on your project requirements). For more details on docker-compose.yml you follow this [docker-compose commands](https://docs.docker.com/engine/reference/commandline/compose/)

```bash
version: "3.4"
services:
 vite_docker:
   image: node:alpine
   container_name: vite_docker
   entrypoint: /bin/sh
   ports:
     - 8000:8000
   working_dir: /src/app
   volumes:
     - type: bind
       source: ./
       target: /src/app
   tty: true
```

