FROM continuumio/miniconda3:4.7.12 
LABEL maintainer="yannc@jfrog.com"

ENV BUILD_NAME='test_conda'
ENV BUILD_NUMBER=1

ENV ARTY_URL='http://192.168.41.41:8081/artifactory'
ENV ARTY_USER='admin'
ENV ARTY_APIKEY='AKCp5btpCwjQnQr3QSZv7XK7i7W7Yf1BXyzdXqsUKPePDHqYE3HLDPHdF9F7spexvso3sNJSN'
ENV ARTY_MAIN_REPO='eva-conda'
ENV ARTY_ID='art6'
ENV PKG_CACHE=/opt/conda/pkgs
ENV JFROG_CLI_OFFER_CONFIG=false

WORKDIR /tmp

RUN apt-get install -y curl git

RUN curl -fL https://getcli.jfrog.io | sh &&  chmod 755 jfrog &&  mv jfrog /usr/local/bin/

RUN jfrog rt c --interactive=false --url=$ARTY_URL --user=$ARTY_USER --apikey=$ARTY_APIKEY $ARTY_ID

RUN git clone https://github.com/cyan21/conda-pipeline.git

RUN chmod u+x conda-pipeline/scripts/init.sh conda-pipeline/scripts/build.sh

RUN conda-pipeline/scripts/init.sh -u $ARTY_USER -k $ARTY_APIKEY -l $ARTY_URL -r $ARTY_MAIN_REPO 

ENTRYPOINT ["/bin/bash", "-c", "cd conda-pipeline && scripts/build.sh -i $BUILD_NAME -n $BUILD_NUMBER -c $PKG_CACHE -r $ARTY_MAIN_REPO -a $ARTY_ID"]
