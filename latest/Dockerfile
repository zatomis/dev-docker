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
