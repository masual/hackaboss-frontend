# Welcome to Hack a Boss Frontend :whale2:

## Introduction

This repository was created with the aim of testing a **Frontend** application (**Vue.js**) with a **Backend** (**Python**) and a **database** (**PostgreSQL**).

## First Steps

* Install **Docker** : [Link](https://docs.docker.com/get-docker/)
* Install **Docker Compose** : [Link](https://docs.docker.com/compose/install/)

## Test Frontend

- Build **Frontend**

```bash
sudo docker build --build-arg GIT_TAG=0.0.1 -t hackaboss-frontend:0.0.1 -f image/Dockerfile .
```

:eyes: Before running **Frontend** application, it is necessary to run **Backend** application and  a **Database**. More information on [hackaboss-backend](https://github.com/masual/hackaboss-backend).

* Run **Frontend**

```bash
sudo docker container run -d --name hackaboss-frontend \
 -p 8080:80 \
 --network ednonlabs-network \
hackaboss-frontend:0.0.1
```
