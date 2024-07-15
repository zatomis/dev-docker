FROM python:3.10.14-slim-bookworm
LABEL author=Max
ARG JINJA_VER="0.1.1"
ARG JINJA_HASH
ARG JINJA_LINK=https://github.com/MrDave/StaticJinjaPlus/archive/refs/tags/$JINJA_VER.tar.gz
WORKDIR /jinja
ADD --checksum=sha256:$JINJA_HASH $JINJA_LINK /tmp/app.tar.gz
RUN tar xzf /tmp/app.tar.gz --strip-components=1 -C /jinja && rm /tmp/app.tar.gz
RUN pip install -r requirements.txt
RUN mv templates_example templates 
ENTRYPOINT ["python3", "main.py"]
