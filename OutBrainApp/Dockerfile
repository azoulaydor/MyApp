# syntax=docker/dockerfile:1
FROM ubuntu:latest

RUN mkdir /opt/myapi

RUN apt-get update \
  && apt-get install -y python3-pip python3-dev \
  && cd /usr/local/bin \
  && ln -s /usr/bin/python3 python \
  && pip3 install --upgrade pip

RUN useradd -m appuser
RUN chmod 775 /opt/myapi \
  && chown root:appuser /opt/myapi

USER appuser
WORKDIR /home/appuser

COPY --chown=appuser:appuser requirements.txt requirements.txt

ENV PATH="/home/appuser/.local/bin:${PATH}"

RUN pip install --user -r requirements.txt

COPY ["main.py", "resources.py", "input.json", "/opt/myapi/"]

WORKDIR /opt/myapi

CMD ["python3", "main.py"]



