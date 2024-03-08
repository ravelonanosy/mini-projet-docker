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

![image](https://github.com/ravelonanosy/mini-projet-docker/assets/138290448/5b581bdc-87d5-48d0-88f1-0563d0ac9d78)


2- create network :

creation of a “bridge type” network to place the 2 containers

docker network create student-list-nw

![image](https://github.com/ravelonanosy/mini-projet-docker/assets/138290448/d7303589-189b-4245-a7ec-b8513136e466)


```

Build your image and try to run it (don't forget to mount *student_age.json* file at */data/student_age.json* in the container), check logs and verify that the container is listening and is  ready to answer

Run this command to make sure that the API correctly responding (take a screenshot for delivery purpose)
```bash 
curl -u toto:python -X GET http://<host IP>:<API exposed port>/pozos/api/v1.0/get_student_ages
```

**Congratulation! Now you are ready for the next step (docker-compose.yml)**

## Infrastructure As Code (5 points)

After testing your API image, you need to put all together and deploy it, using docker-compose.

The ***docker-compose.yml*** file will deploy two services :

- website: the end-user interface with the following characteristics
   - image: php:apache
   - environment: you will provide the USERNAME and PASSWORD to enable the web app to access the API through authentication
   - volumes: to avoid php:apache image run with the default website, we will bind the website given by POZOS to use. You must have something like
`./website:/var/www/html`
   - depend on: you need to make sure that the API will start first before the website
   - port: do not forget to expose the port
- API: the image builded before should be used with the following specification
   - image: the name of the image builded previously
   - volumes: You will mount student_age.json file in /data/student_age.json
   - port: don't forget to expose the port
   - networks: don't forget to add specific network for your project

Delete your previous created container

Run your docker-compose.yml

Finally, reach your website and click on the bouton "List Student"

**If the list of the student appears, you are successfully dockerizing the POZOS application! Congratulation (make a screenshot)**

## Docker Registry (4 points)

POZOS need you to deploy a private registry and store the built images

So you need to deploy :

- a docker [registry](https://docs.docker.com/registry/ "registry")
- a web [interface](https://hub.docker.com/r/joxit/docker-registry-ui/ "interface") to see the pushed image as a container

Or you can use [Portus](http://port.us.org/ "Portus") to run both

Don't forget to push your image on your private registry and show them in your delivery.

## Delivery (4 points)

Your delivery must be zip named firstname.zip (replace firstname by your own) that contain:

- A doc or PDF file with your screenshots and explanations.
- Configuration files used to realize the graded exercise (docker-compose.yml and Dockerfile).

Your delivery will be evaluated on:

- Explanations quality
- Screenshots quality (relevance, visibility)
- Presentation quality

Send your delivery at ***eazytrainingfr@gmail.com*** and we will provide you the link of the solution.

![good luck](https://user-images.githubusercontent.com/18481009/84582398-cad38100-adeb-11ea-95e3-2a9d4c0d5437.gif)
