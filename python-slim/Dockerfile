FROM python:3.10.14-slim-bookworm
LABEL author=Max
RUN apt-get update &&  apt-get install git -y && rm -rf /var/lib/apt/lists/*
WORKDIR /"jinja"
ADD https://github.com/MrDave/StaticJinjaPlus.git .
RUN pip install -r requirements.txt
RUN mv templates_example templates 
ENTRYPOINT ["python3", "main.py"]
