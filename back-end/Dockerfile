FROM ubuntu:22.04

RUN apt-get update
RUN apt-get -y install curl

RUN apt-get -y install gcc-11
RUN apt-get -y install g++-11

RUN apt-get -y install python3
RUN apt-get -y install python3-pip
RUN apt-get -y install python3-venv

ENV TZ=Europe/Kyiv
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
RUN apt-get -y install mono-mcs

RUN curl -fsSL https://deb.nodesource.com/setup_20.x | bash -
RUN apt-get -y install nodejs

RUN python3 -m venv back-end-venv

COPY Connector/__MAIN__/requirements.txt requirements.txt

RUN /back-end-venv/bin/pip install -r requirements.txt

COPY Connector/__MAIN__ Connector/__MAIN__

COPY Checker Checker

WORKDIR /Connector/__MAIN__
RUN npm install efrog@latest
RUN npm update

USER root
RUN chmod -R go-r /Connector/__MAIN__
RUN chmod -R go+r /Connector/__MAIN__/node_modules

EXPOSE 8000
EXPOSE 3306

CMD ["/back-end-venv/bin/uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]