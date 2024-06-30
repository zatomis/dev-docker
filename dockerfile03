FROM ubuntu:22.04
LABEL author=Max
RUN apt-get update && apt install -y git python3.10 python3-pip python3-venv && rm -rf /var/lib/apt/lists/*
WORKDIR /"jinja"
RUN git clone https://github.com/MrDave/StaticJinjaPlus.git
WORKDIR /jinja/StaticJinjaPlus
RUN pip install -r requirements.txt
RUN mv templates_example templates 
ENTRYPOINT ["python3", "main.py"]
