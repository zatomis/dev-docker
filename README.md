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
ARG JINJA_LINK=https://github.com/MrDave/StaticJinjaPlus/archive/refs/tags/$JINJA_VER.tar.gz
RUN apt update && apt install -y curl python3.10 python3-pip && rm -rf /var/lib/apt/lists/*
WORKDIR /"jinja"
ADD $JINJA_LINK site
RUN online_md5="$(curl -sL $JINJA_LINK | md5sum | cut -d ' ' -f 1)" && echo $online_md5
RUN local_md5="$(md5sum "site" | cut -d ' ' -f 1)" && echo $local_md5
RUN if [ "$online" = "$local_md5" ]; then echo "Файл $JINJA_LINK прошел проверку на подлинность !!!"; else exit 1; fi
RUN tar -xvf site && rm site
RUN mv "StaticJinjaPlus-"$JINJA_VER StaticJinjaPlus 
WORKDIR /jinja/StaticJinjaPlus
RUN pip install -r requirements.txt
RUN mv templates_example templates 
ENTRYPOINT ["python3", "main.py"]
```
данный файл создает образы для 
- 0.1.0-ubuntu
- 0.1.1-ubuntu

где в качестве параметрa JINJA_VER - № тега сборки 

```shell
docker build -f dockerfile01 --progress=plain --no-cache -t ubuntu_staticjinjaplus:v1 --build-arg JINJA_VER="0.1.0" .
```

```shell
docker build -f dockerfile01 --progress=plain --no-cache -t ubuntu_staticjinjaplus:v2 --build-arg JINJA_VER="0.1.1" .
```

Следующий файл применим для сборок
- 0.1.0-slim
- 0.1.1-slim

```
FROM python:3.10.14-slim-bookworm
LABEL author=Max
ARG JINJA_VER="0.1.1"
WORKDIR /jinja
ADD https://github.com/MrDave/StaticJinjaPlus/archive/refs/tags/$JINJA_VER.tar.gz site
RUN tar -xvf site && rm site
RUN mv "StaticJinjaPlus-"$JINJA_VER StaticJinjaPlus 
WORKDIR /jinja/StaticJinjaPlus
RUN pip install -r requirements.txt
RUN mv templates_example templates 
ENTRYPOINT ["python3", "main.py"]
```
где в качестве параметров - теги сборок 
```shell
docker build -f dockerfile02 --progress=plain --no-cache -t slim_staticjinjaplus:v3 --build-arg JINJA_VER="0.1.0" .
```
```shell
docker build -f dockerfile02 --progress=plain --no-cache -t slim_staticjinjaplus:v4 --build-arg JINJA_VER="0.1.1" .
```

Для сборки последней актуальной версии используем скрипт :
для - latest-ubuntu
```
FROM ubuntu:22.04
LABEL author=Max
RUN apt-get update && apt install -y git python3.10 python3-pip python3-venv && rm -rf /var/lib/apt/lists/*
WORKDIR /"jinja"
RUN git clone https://github.com/MrDave/StaticJinjaPlus.git
WORKDIR /jinja/StaticJinjaPlus
RUN pip install -r requirements.txt
RUN mv templates_example templates 
ENTRYPOINT ["python3", "main.py"]
```

```shell
docker build -f dockerfile03 --progress=plain --no-cache -t ubuntu_staticjinjaplus:latest .
```

для - latest-slim
```
FROM python:3.10.14-slim-bookworm
LABEL author=Max
RUN apt-get update
RUN apt-get install git -y && rm -rf /var/lib/apt/lists/*
WORKDIR /"jinja"
RUN git clone https://github.com/MrDave/StaticJinjaPlus.git
WORKDIR /jinja/StaticJinjaPlus
RUN pip install -r requirements.txt
RUN mv templates_example templates 
ENTRYPOINT ["python3", "main.py"]
```

```shell
docker build -f dockerfile04 --progress=plain --no-cache -t slim_staticjinjaplus:latest .
```

- latest
```
FROM python:3.10
LABEL author=Max
RUN apt-get update
RUN apt-get install git -y && rm -rf /var/lib/apt/lists/*
WORKDIR /"jinja"
RUN git clone https://github.com/MrDave/StaticJinjaPlus.git
WORKDIR /jinja/StaticJinjaPlus
RUN pip install -r requirements.txt
RUN mv templates_example templates 
ENTRYPOINT ["python3", "main.py"]
```
```shell
docker build -f dockerfile05 --progress=plain --no-cache -t python_staticjinjaplus:latest .
```

# Запуск образа
Воспользуйтесь командой ниже. `Запуск всех образов аналогичен, за исключение указания номера версии сборки образа`
```shell
docker run -v "$(pwd)"/site:/jinja/StaticJinjaPlus/build --rm -it django:v [1,2,3]->указать номер версии образа
```
После выполнения в текущем каталоге появится папка site с готовым шаблоном 


## Цели проекта

Код написан в учебных целях. Cайт [Devman](https://dvmn.org). 
