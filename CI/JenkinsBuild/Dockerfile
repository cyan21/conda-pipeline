FROM continuumio/miniconda3:4.7.12 
LABEL maintainer="yannc@jfrog.com"

ARG ARTY_URL
ARG ARTY_USER
ARG ARTY_APIKEY 
ARG ARTY_ID
ARG PKG_CACHE=/opt/conda/pkgs
ENV JFROG_CLI_OFFER_CONFIG=false
ENV JFROG_CLI_HOME=/opt/jfrog 
ENV HOME=/opt/jfrog 

RUN mkdir /opt/jfrog
WORKDIR /opt/jfrog

RUN apt-get install -y curl 

RUN curl -fL https://getcli.jfrog.io | sh &&  chmod 755 jfrog &&  mv jfrog /usr/local/bin/

RUN jfrog rt c --interactive=false --url=$ARTY_URL --user=$ARTY_USER --apikey=$ARTY_APIKEY $ARTY_ID

RUN jfrog rt use $ARTY_ID

# hack
RUN mkdir -p /opt/conda/pkgs && chmod 777 -R /opt/conda/ /opt/jfrog

CMD ["/bin/bash", "-c", "jfrog rt c show"]

