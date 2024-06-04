# Докеризируем генератор статических сайтов 

### Установка docker

[Установите Docker](https://docs.docker.com/engine/install/ubuntu/).

Проверьте, что `docker` установлен и корректно настроен. Запустите его в командной строке:
```shell
docker --version
```

### Образец готового образа на [Docker hub](https://hub.docker.com/r/zatomis/static-jinja-plus).
Так же вы можете сами собрать образы. 7 вариантов установок различных установок
Создайте докер файл
```shell
touch dockerfile
```
с содержанием :
```
FROM ubuntu:22.04
LABEL author=Max
ARG JINJA_VER="0.1.1"
RUN apt-get update
RUN apt-get install wget -y
RUN apt-get install python3.10 -y
RUN apt-get install zip -y
RUN apt-get install python3-pip -y
RUN apt install -y build-essential libssl-dev libffi-dev python3-dev
RUN apt install -y python3-venv
WORKDIR /"jinja"_$JINJA_VER
RUN wget https://github.com/MrDave/StaticJinjaPlus/archive/refs/tags/$JINJA_VER.zip
RUN unzip $JINJA_VER.zip
RUN rm $JINJA_VER.zip
WORKDIR /"jinja"_$JINJA_VER/"StaticJinjaPlus-"$JINJA_VER
RUN python3.10 -m venv venv
RUN . venv/bin/activate && pip install -r requirements.txt
RUN mv templates_example templates 
```
данный файл создает образы для 
- 0.1.0-ubuntu
- 0.1.1-ubuntu
где в качестве параметров - теги сборок 
docker build -t ubuntu_docker:v01 --build-arg JINJA_VER="0.1.0" .
docker build -t ubuntu_docker:v02 --build-arg JINJA_VER="0.1.1" .

Следующий файл применим для сборок
- 0.1.0-slim
- 0.1.1-slim
где в качестве параметров - теги сборок 
```
FROM python:3.10.14-slim-bookworm
LABEL author=Max
ARG JINJA_VER="0.1.1"
RUN apt-get update
RUN apt-get install wget -y
RUN apt-get install zip -y
WORKDIR /"jinja"_$JINJA_VER
RUN wget https://github.com/MrDave/StaticJinjaPlus/archive/refs/tags/$JINJA_VER.zip
RUN unzip $JINJA_VER.zip
RUN rm $JINJA_VER.zip
WORKDIR /"jinja"_$JINJA_VER/"StaticJinjaPlus-"$JINJA_VER
RUN python3.10 -m venv venv
RUN . venv/bin/activate && pip install -r requirements.txt
RUN mv templates_example templates 
```

- latest-ubuntu
- latest-slim
- latest


## Цели проекта

Код написан в учебных целях — это урок в курсе по Python и веб-разработке на сайте [Devman](https://dvmn.org). 
