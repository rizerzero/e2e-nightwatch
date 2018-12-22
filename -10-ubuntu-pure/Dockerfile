FROM ubuntu:16.04

## Install dependencies: nodejs, chrome and xvfb


RUN \
  curl -sL https://deb.nodesource.com/setup_6.x | bash - && \
  wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add - && \
  echo "deb http://dl.google.com/linux/chrome/deb/ stable main" > /etc/apt/sources.list.d/google.list && \
  apt-get update && \
  apt-get install -y nodejs google-chrome-stable xvfb && \
  npm install -g nightwatch && \
  rm -rf /var/lib/apt/lists/*
# install java
# disable interactive functions
ENV DEBIAN_FRONTEND noninteractive

# set default java environment variable
ENV JAVA_VERSION_MAJOR=8 \
    JAVA_VERSION_MINOR=111 \
    JAVA_HOME=/usr/lib/jvm/default-jvm \
    PATH=${PATH}:/usr/lib/jvm/default-jvm/bin/

RUN add-apt-repository ppa:openjdk-r/ppa -y && \

    # update data from repositories
    apt-get update && \

    # upgrade OS
    apt-get -y dist-upgrade && \

    # Make info file about this build
    printf "Build of julesbel/java:openjdk-8-jre-headless, date: %s\n"  `date -u +"%Y-%m-%dT%H:%M:%SZ"` > /etc/BUILDS/java && \

    # install application
    apt-get install -y --no-install-recommends openjdk-8-jre-headless && \

    # fix default setting
    ln -s java-8-openjdk-amd64  /usr/lib/jvm/default-jvm && \

    # remove apt cache from image
    apt-get clean all

WORKDIR /tests
## Fix for selenium issue, see: https://github.com/SeleniumHQ/docker-selenium/issues/87
ENV DBUS_SESSION_BUS_ADDRESS=/dev/null

## Run nightwatch tests using xvfb
CMD xvfb-run --server-args="-screen 0 1600x1200x24" nightwatch


# ----------------
# ----------------
# Install Node 8 (LTS)
# ----------------
#RUN curl -sL https://deb.nodesource.com/setup_8.x | bash - \ && apt-get install -y install nodejs \ && npm install npm@latest -g
# ENV NODE_VERSION=8.11.3
# RUN wget -q -O node-v$NODE_VERSION-linux-x64.tar.xz https://nodejs.org/dist/v$NODE_VERSION/node-v$NODE_VERSION-linux-x64.tar.xz
# RUN tar -xJf node-v$NODE_VERSION-linux-x64.tar.xz -C /usr/local --strip-components=1
# RUN rm node-v$NODE_VERSION-linux-x64.tar.xz
# RUN ln -s /usr/local/bin/node /usr/local/bin/nodejs
 