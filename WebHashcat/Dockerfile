FROM ubuntu:focal

RUN apt-get update && \
    apt-get install -y python3 python3-pip libmysqlclient-dev && \
	pip3 install --upgrade pip

WORKDIR /

RUN apt-get update && \
    apt-get install -y wget make clinfo build-essential git libcurl4-openssl-dev libssl-dev zlib1g-dev libcurl4-openssl-dev libssl-dev

RUN apt-get install -y libsasl2-dev python-dev-is-python3 libldap2-dev

ENV HASHCAT_VERSION master
RUN git clone https://github.com/hashcat/hashcat.git && cd hashcat && git checkout ${HASHCAT_VERSION} && make install -j4

#FlL: pas envie de recompiler hashcat
ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && \
    apt-get install -y pkg-config && \
    rm -rf /var/lib/apt/lists/*
# FlL /off

WORKDIR /webhashcat


# Installing requirements
COPY requirements.txt requirements.txt 
RUN pip3 install -r requirements.txt

# Copying files
COPY . .
RUN chmod +x /webhashcat/entrypoint.sh
#RUN chmod 777 /webhashcat/Files/tmp # FlL removed, Files/ is .dockerignore now 

# Preparing configuration
RUN mv /webhashcat/WebHashcat/settings.py.docker /webhashcat/WebHashcat/settings.py
RUN mv /webhashcat/settings.ini.docker /webhashcat/settings.ini

# Entrypoint
ENTRYPOINT ["/webhashcat/entrypoint.sh"]

