FROM python:3.10.14-slim-bookworm
ARG JINJA_HASH
ARG JINJA_VER
ARG JINJA_LINK=https://github.com/MrDave/StaticJinjaPlus/archive/refs/tags/$JINJA_VER.tar.gz
WORKDIR /jinja
ADD --checksum=sha256:$JINJA_HASH $JINJA_LINK /tmp/app.tar.gz
RUN tar xzf /tmp/app.tar.gz --strip-components=1 -C /jinja && rm /tmp/app.tar.gz
RUN pip install --no-cache-dir -r requirements.txt
RUN mv templates_example templates 
ENTRYPOINT ["python3", "main.py"]
