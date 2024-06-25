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
RUN apt update && apt install -y python3.10 python3-pip zip python3-venv
WORKDIR /"jinja"
ADD https://github.com/MrDave/StaticJinjaPlus/archive/refs/tags/$JINJA_VER.zip site_zip
RUN unzip site_zip && rm site_zip
RUN mv "StaticJinjaPlus-"$JINJA_VER StaticJinjaPlus 
WORKDIR /jinja/StaticJinjaPlus
RUN python3.10 -m venv venv
RUN . venv/bin/activate && pip install -r requirements.txt
RUN mv templates_example templates 
ENTRYPOINT ["venv/bin/python3.10", "main.py"]
```
данный файл создает образы для 
- 0.1.0-ubuntu
- 0.1.1-ubuntu

где в качестве параметрa JINJA_VER - № тега сборки 

```shell
docker build -f dockerfile01 -t ubuntu_staticjinjaplus:v1 --build-arg JINJA_VER="0.1.0" .
```

```shell
docker build -f dockerfile01 -t ubuntu_staticjinjaplus:v2 --build-arg JINJA_VER="0.1.1" .
```

Следующий файл применим для сборок
- 0.1.0-slim
- 0.1.1-slim

```
FROM python:3.10.14-slim-bookworm
LABEL author=Max
ARG JINJA_VER="0.1.1"
RUN apt-get update && apt install -y zip
WORKDIR /jinja
ADD https://github.com/MrDave/StaticJinjaPlus/archive/refs/tags/$JINJA_VER.zip site_zip
RUN unzip site_zip && rm site_zip
RUN mv "StaticJinjaPlus-"$JINJA_VER StaticJinjaPlus 
WORKDIR /jinja/StaticJinjaPlus
RUN python3.10 -m venv venv
RUN . venv/bin/activate && pip install -r requirements.txt
RUN mv templates_example templates 
ENTRYPOINT ["venv/bin/python3.10", "main.py"]

```
где в качестве параметров - теги сборок 
```shell
docker build -f dockerfile02 -t slim_staticjinjaplus:v3 --build-arg JINJA_VER="0.1.0" .
```
```shell
docker build -f dockerfile02 -t slim_staticjinjaplus:v4 --build-arg JINJA_VER="0.1.1" .
```

Для сборки последней актуальной версии используем скрипт :
для - latest-ubuntu
```
FROM ubuntu:22.04
LABEL author=Max
RUN apt-get update && apt install -y git python3.10 python3-pip python3-venv
#RUN apt install -y build-essential libssl-dev libffi-dev python3-dev
WORKDIR /"jinja"
RUN git clone https://github.com/MrDave/StaticJinjaPlus.git
WORKDIR /jinja/StaticJinjaPlus
RUN python3.10 -m venv venv
RUN . venv/bin/activate && pip install -r requirements.txt
RUN mv templates_example templates 
ENTRYPOINT ["venv/bin/python3.10", "main.py"]
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
ENTRYPOINT ["venv/bin/python3.10", "main.py"]
```

```shell
docker build -f dockerfile04 -t slim_staticjinjaplus:latest .
```

- latest
```
FROM python:3.10
LABEL author=Max
RUN apt-get update
RUN apt-get install git -y
WORKDIR /"jinja"
RUN git clone https://github.com/MrDave/StaticJinjaPlus.git
WORKDIR /jinja/StaticJinjaPlus
RUN python3.10 -m venv venv
RUN . venv/bin/activate && pip install -r requirements.txt
RUN mv templates_example templates 
ENTRYPOINT ["venv/bin/python3.10", "main.py"]
```
```shell
docker build -f dockerfile05 -t python_staticjinjaplus:latest .
```

# Запуск образа
Воспользуйтесь командой ниже. `Запуск всех образов аналогичен, за исключение указания номера версии сборки образа`
```shell
docker run -v "$(pwd)"/site:/jinja/StaticJinjaPlus/build --rm -it django:v [1,2,3]->указать номер версии образа
```
После выполнения в текущем каталоге появится папка site с готовым шаблоном 


## Цели проекта

Код написан в учебных целях. Cайт [Devman](https://dvmn.org). 
