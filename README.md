# Докеризируем генератор статических сайтов 

### Установка docker

[Установите Docker](https://docs.docker.com/engine/install/ubuntu/).

Проверьте, что `docker` установлен и корректно настроен. Запустите его в командной строке:
```shell
docker --version
```

### Образец готового образа доступен на [Docker hub](https://hub.docker.com/r/zatomis/static-jinja-plus).
Так же вы можете сами собрать образы.
7 вариантов установок различных установок

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
где в качестве параметрa JINJA_VER - № тега сборки 
```shell
docker build -f dockerfile01 -t ubuntu_staticjinjaplus:v01 --build-arg JINJA_VER="0.1.0" .
```
```shell
docker build -f dockerfile01 -t ubuntu_staticjinjaplus:v02 --build-arg JINJA_VER="0.1.1" .
```

```shell
touch dockerfile
```
Следующий файл применим для сборок
- 0.1.0-slim
- 0.1.1-slim

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
где в качестве параметров - теги сборок 
```shell
docker build -f dockerfile02 -t slim_staticjinjaplus:v03 --build-arg JINJA_VER="0.1.0" .
```
```shell
docker build -f dockerfile02 -t slim_staticjinjaplus:v04 --build-arg JINJA_VER="0.1.1" .
```


Для сборки последней актуальной версии используем скрипт :
для - latest-ubuntu
```
FROM ubuntu:22.04
LABEL author=Max
RUN apt-get update
RUN apt-get install git -y
RUN apt-get install python3.10 -y
RUN apt-get install python3-pip -y
RUN apt install -y build-essential libssl-dev libffi-dev python3-dev
RUN apt install -y python3-venv
WORKDIR /"jinja"
RUN git clone https://github.com/MrDave/StaticJinjaPlus.git
WORKDIR /jinja/StaticJinjaPlus
RUN python3.10 -m venv venv
RUN . venv/bin/activate && pip install -r requirements.txt
RUN mv templates_example templates 
```
```shell
docker build -f dockerfile03 -t ubuntu_staticjinjaplus:latest .
```

для - latest-slim
```
FROM python:3.10.14-slim-bookworm
LABEL author=Max
RUN apt-get update
RUN apt-get install git -y
WORKDIR /"jinja"
RUN git clone https://github.com/MrDave/StaticJinjaPlus.git
WORKDIR /jinja/StaticJinjaPlus
RUN python3.10 -m venv venv
RUN . venv/bin/activate && pip install -r requirements.txt
RUN mv templates_example templates 
```
```shell
docker build -f dockerfile04 -t slim_staticjinjaplus:latest .
```

- latest


## Цели проекта

Код написан в учебных целях. Cайт [Devman](https://dvmn.org). 
