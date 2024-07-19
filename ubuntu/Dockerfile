FROM ubuntu:22.04
LABEL author=Max
RUN apt-get update && apt-get install -y git python3.10 python3-pip python3-venv && rm -rf /var/lib/apt/lists/*
WORKDIR /"jinja"
ADD https://github.com/MrDave/StaticJinjaPlus.git .
RUN pip install --no-cache-dir -r requirements.txt
RUN mv templates_example templates 
ENTRYPOINT ["python3", "main.py"]
