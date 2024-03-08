# student-list 
This repo is a simple application to list student with a webserver (PHP) and API (Flask)

![project](https://user-images.githubusercontent.com/18481009/84582395-ba230b00-adeb-11ea-9453-22ed1be7e268.jpg)


------------



## Context

*POZOS*  is an IT company located in France and develops software for High School.

The innovation department want to disrupt the existing infrastructure to ensure that

it can be scalable, easily deployed with a maximum of automation.

POZOS wants you to build a **POC** to show how docker can help you and how much this technology is efficient.

For this POC, POZOS will give you an application and want you to build a "decouple" infrastructure based on **Docker**.

Currently, the application is running on a single server with any scalability and any high availability.

When POZOS needs to deploy a new release, every time some goes wrong.

In conclusion, POZOS needs agility on its software farm.


## Application


The application that you will be working on is named "*student_list*", this application is very basic and enables POZOS to show the list of the student with their age.

student_list has two modules:

- the first module is a REST API (with basic authentication needed) who send the desire list of the student based on JSON file
- The second module is a web app written in HTML + PHP who enable end-user to get a list of students

## The need


To run this "student-list" application, my work is focused on:
- deployment of these 2 modules in docker containers which are interacting
- archiving images docker in a private registry

## Implementation
infrastructure:
A Centos 7 virtual machine including the docker package was deployed locally on virtualbox with vagrant.


application files:
- docker-compose.yml: to launch the application
- docker-compose-registry.yml : to launch private registry application
- simple_api/Dockerfile: the file that will be used to build the API image 
- simple_api/requirements.txt: contains all the packages to be installed to run the application
- simple_api/student_age.json: contain student name with age on JSON format
- simple_api/student_age.py: contains the source code of the API in python
- website/index.php: PHP  page where end-user will be connected to interact with the service to - list students with age.



## Build and test

Here are the different technical stages of the project:

1- build images docker:

docker build -t flask-nra:v3 .

docker images

![image](https://github.com/ravelonanosy/mini-projet-docker/assets/138290448/7661dac8-20bf-4b72-88bc-8758746f92aa)

2- create network :

creation of a “bridge type” network to place the 2 containers

docker network create student-list-nw

![image](https://github.com/ravelonanosy/mini-projet-docker/assets/138290448/e4a747fc-8a32-431a-815f-9eee0cb2d7a3)

3-create containers for local registry using docker-compose:

docker compose -f docker-compose-registry.yml up -d

docker ps -a |grep registry

![image](https://github.com/ravelonanosy/mini-projet-docker/assets/138290448/c1b76e0d-9803-49d2-b800-2ef396b1c532)

4-After testing the image with a "docker run" command line, the image was then sent to the local registry:

docker tag 4b865a5312ca localhost:5000/flask-nra:local-v3

docker push localhost:5000/flask-nra:local-v3

![image](https://github.com/ravelonanosy/mini-projet-docker/assets/138290448/a215442f-f78d-4f62-a971-ec487c06535a)


![image](https://github.com/ravelonanosy/mini-projet-docker/assets/138290448/85b47558-cd94-4b06-99ff-f37b3afcc4cf)


5- create application'containers using docker-compose and the image on the local registry :

docker compose -f docker-compose.yml up -d

docker ps -a

![image](https://github.com/ravelonanosy/mini-projet-docker/assets/138290448/93b26d43-57b2-495f-bb82-d4e160da1647)

6- test curl of API:

![image](https://github.com/ravelonanosy/mini-projet-docker/assets/138290448/0c3a178f-3a3d-499c-ba3b-e07661fc3629)

7- 

![image](https://github.com/ravelonanosy/mini-projet-docker/assets/138290448/7fd946d1-7cb7-4a8d-9c72-349756852c58)




![good luck](https://user-images.githubusercontent.com/18481009/84582398-cad38100-adeb-11ea-95e3-2a9d4c0d5437.gif)
