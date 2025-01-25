 
# Python App (Flask) with MySQL Docker Setup

This is a simple Flask app that interacts with a MySQL database. The app allows users to submit messages, which are then stored in the database and displayed on the frontend.

## Prerequisites

Before you begin, make sure you have the following installed:

- Docker
- Git (optional, for cloning the repository)

## Setup

1. Clone this repository (if you haven't already):

   ```bash
   git clone https://github.com/Noufalnazr/Docker_App.git
   ```

- First CD to the root folder and make sure the Dockerfile and other files are avaialbe on your cloned folder 
- create a docker image from Dockerfile
```bash
sudo docker build -t pythonbackend .
```

- Now, make sure that you have created a network using following command
```bash
sudo docker network create net-pythonapp
```

- Attach both the containers in the same network, so that they can communicate with each other

i) Create the MySQL container(This will install from DockerHub) 
```bash
sudo docker run -d \
    --name mysql \
    -v mysql-data:/var/lib/mysql \
    --network=net-pythonapp \
    -e MYSQL_DATABASE=mydb \
    -e MYSQL_ROOT_PASSWORD=admin \
    -p 3306:3306 \
    mysql:5.7

```
ii) create python Backend container
```bash
sudo docker run -d \
    --name pythonapp \
    --network=net-pythonapp \
    -e MYSQL_HOST=mysql \
    -e MYSQL_USER=root \
    -e MYSQL_PASSWORD=admin \
    -e MYSQL_DB=mydb \
    -p 5000:5000 \
    pythonbackend:latest

```

