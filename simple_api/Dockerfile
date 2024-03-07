FROM python:3.8-slim-buster

RUN apt-get update
#RUN apt-get -y install gcc
RUN DEBIAN_FRONTEND=noninteractive apt-get -y install build-essential python3-dev libldap2-dev libsasl2-dev slapd ldap-utils tox lcov valgrind

WORKDIR /usr/src/app

COPY requirements.txt /usr/src/app/

RUN pip3 install -r /usr/src/app/requirements.txt

COPY . /usr/src/app/


CMD ["python", "student_age.py"]
