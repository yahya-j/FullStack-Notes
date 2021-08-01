# FullStack-Notes

This repository holds the code for :
- fullstack-notes-application - A full-stack CRUD application with [nginx](https://hub.docker.com/_/nginx/) as a reverse proxy.


## Prerequisites

- Familiarity with the Linux Terminal.
- Familiarity with JavaScript (some of the later projects use JavaScript).

It's fine if you haven't worked with JavaScript that much. Having a basic knowledge of executing scripts with `npm` will suffice.

## Docker

Instead of writing so many commands, there is an easier way to manage multi-container projects, a tool called Docker Compose.
According to the Docker documentation - "Compose is a tool for defining and running multi-container Docker applications. 
With Compose, you use a YAML file to configure your application’s services. Then, with a single command, you create and start 
all the services from your configuration."
Although Compose works in all environments, it's more focused on development and testing. 
| :bell: NOTIFICATION |
|:--------------------|
| Using Compose on a production environment is not recommended at all. 
| Instead, use K8s for container's orchestration is better. |

Before you execute the command :    docker-compose --file docker-compose.yaml up --detach
Make sure you've opened your terminal in the same directory where the docker-compose.yaml file is. 
This is very important for every docker-compose command you execute.

### docker-compose.yaml file
version: "3.8"

services: 
    db:
        image: postgres:12
        container_name: notes-db-dev
        volumes: 
            - db-data:/var/lib/postgresql/data
        environment:
            POSTGRES_DB: notesdb
            POSTGRES_PASSWORD: secret
    api:
        build:
            context: ./api
            dockerfile: Dockerfile.dev
        image: notes-api:dev
        container_name: notes-api-dev
        environment: 
            DB_HOST: db ## same as the database service name
            DB_DATABASE: notesdb
            DB_PASSWORD: secret
        volumes: 
            - /home/node/app/node_modules
            - ./api:/home/node/app
        ports: 
            - 3000:3000

volumes:
    db-data:
        name: notes-db-dev-data


