# TP-1 Biuld and Deploy pipeline #

The main objective of this work is to know how to build an application based on a Docker container, deploy it and manage it in its execution environment using Jenkins and learn about collaborative working using Git.

## Requirements ##

* Jenkins (jenkins/jenkins:lts-jdk17) with Docker Pipeline  

## Instructions for carrying out the work ##

### 1 - Retrieve the source code repository locally on your virtual machine with the command

```console
git clone https://bitbucket.org/efrei2023/practical-exercises.git
```

### 2 - Prepare Jenkins

* Add/Register credentials for git (source code) repository and docker registry.
  * Git : <https://bitbucket.org/efrei2023/practical-exercises.git>
    * username: efrei_2023
    * password: ATBBzvHf2cYPLnq8hnreNm52Un5wBB6FF145
  * Docker Hub : efrei2023/devops-tp1
    * username: efrei2023
    * password: efrei2023
* Create a network for docker infrastructure:

   ```console
     docker network create --driver bridge efrei
   ```

### Git repository structure

* backend : A PostgreSQL database consisting of one table with some students (see bakend/database.sql).

* frontend : The webapp that retrieves the students from the database and shows them in a HTML page. This webapp is build using Flask (a Python Microframework).

## Exercise

## 1. Building the backend and frontend

* Build by using a Dockerfile
  * Analyze the contents of the 'backend/Dockerfile' and 'frontend/Dockerfile files

  * Go to the 'backend' directory and type the command :

   ```console
     docker build -t backend-$MAME:YEAR_OF_BIRTH .
   ```

  * Go to the 'frontend' directory and type the command :

   ```console
     docker build -t frontend-$MAME:YEAR_OF_BIRTH .
   ```

* Build frontend by using Buildpack

## 2. Running the backend and frontend

* Run the 'backend' container with the following  command :

   ```console
     docker run -d --net=efrei -e POSTGRES_PASSWORD=postgres --name backend backend-$MAME:YEAR_OF_BIRTH
   ```

* Run the 'frontend' container with the following  command :

   ```console
     docker run -d --net=efrei  -p 8081:80 -e POSTGRES_PASSWORD=postgres --name frontend frontend-$MAME:YEAR_OF_BIRTH
   ```

To make sure your container is up and running, check the logs with the command:

  ```console
     docker logs frontend-$MAME:YEAR_OF_BIRTH
     docker logs backend-$MAME:YEAR_OF_BIRTH
   ```
