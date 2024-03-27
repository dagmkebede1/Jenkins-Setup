# Steps to SetUp Jenkins for Dev and Prod Env.

## Using Docker and the most Recommended way.

## 1. Creating the Dockerfile

`Dockerfile`

```
FROM jenkins/jenkins:2.440.2-jdk17
USER root
RUN apt-get update && apt-get install -y lsb-release
RUN curl -fsSLo /usr/share/keyrings/docker-archive-keyring.asc \
  https://download.docker.com/linux/debian/gpg
RUN echo "deb [arch=$(dpkg --print-architecture) \
  signed-by=/usr/share/keyrings/docker-archive-keyring.asc] \
  https://download.docker.com/linux/debian \
  $(lsb_release -cs) stable" > /etc/apt/sources.list.d/docker.list
RUN apt-get update && apt-get install -y docker-ce-cli
USER jenkins
RUN jenkins-plugin-cli --plugins "blueocean docker-workflow"

```

## 2. Building the Docker Image

Navigate to the directory containing your Dockerfile

```
    cd /path/to/dockerfile/directory
```

Build the Docker image (replace 'myjenkins' with your desired image name)

```
    docker build -t myjenkins .
```

## 3. Running the Docker Container

Run the Docker container (replace 'myjenkins-container' with your desired container name)
-- note : save the password generated on the build process

```
    docker run -d -p 8080:8080 --name myjenkins-container myjenkins
```

## 4. Accessing Jenkins via Web Browser

```
    http://localhost:8080
```

## 5. Signing Up for Jenkins Account

1. Once Jenkins is accessed via the web browser, the Jenkins initial setup screen will be displayed.
2. Follow the on-screen instructions to complete the initial setup, including creating an admin user account, don't forget to provide the password you got up on the container running from the jenkins.
3. Provide the required information, such as username, password, and email address, to register for a Jenkins account.
4. Complete the registration process and log in to Jenkins using the newly created account credentials.

## 6. Additional Notes:

Stop the container

```
    docker stop myjenkins-container
```

Remove the container

```
    docker rm myjenkins-container
```

# Options to manage it using the docker compose

## 1. Creating Docker Compose Configuration

```
# docker-compose.yml
version: '3'

services:
  jenkins:
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - "8080:8080"
```

## 2. Building and Running the Docker Container with Docker Compose

Navigate to the directory containing your Dockerfile and docker-compose.yml

```
    cd /path/to/dockerfile/directory
```

Build and run the Docker container using Docker Compose

```
    docker-compose up -d
```

## 3. Accessing Jenkins via Web Browser

```
    http://localhost:8080

```

the rest steps are the same with setting it with docker.

## 4. Additional Notes:

Stop and remove the Docker Compose services

```
    docker-compose down
```
