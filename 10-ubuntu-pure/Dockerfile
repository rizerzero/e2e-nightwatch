FROM ubuntu:16.04

## Install dependencies: nodejs, chrome and xvfb
RUN apt-get install -yqq openjdk-8-jre-headless


RUN \
  curl -sL https://deb.nodesource.com/setup_10.x | bash - && \
  wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add - && \
  echo "deb http://dl.google.com/linux/chrome/deb/ stable main" > /etc/apt/sources.list.d/google.list && \
  apk update && \
  apt-get install -y nodejs google-chrome-stable xvfb && \
  npm install -g nightwatch && \
  rm -rf /var/lib/apt/lists/*

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
 