FROM ubuntu:16.04
# ----------------
# Prevent a lot of complaining from apt
# ----------------
ENV DEBIAN_FRONTEND noninteractive
# ----------------
# Get everything up to date
# ----------------
RUN apt-get -qq update
# RUN apt-get -yqq upgrade

# ----------------
# Prevent more complaining from apt
# ----------------
RUN apt-get install -yqq apt-utils

# ----------------
# Allow us to download and unpack things
# ----------------
RUN apt-get install -yqq curl
RUN apt-get install -yqq wget
RUN apt-get install -yqq git
RUN apt-get install -yqq bzip2
RUN apt-get install -yqq xvfb
RUN apt-get install -yqq xz-utils
RUN apt-get install -yqq unzip


# ----------------
RUN apt-get install -yqq ca-certificates

# ----------------
# Install Java 8 (needed by selenium)
# ----------------
RUN apt-get install -yqq openjdk-8-jre-headless

# ----------------
# Allow us to parse json and xml data
# ----------------
RUN apt-get install -yqq jq
RUN apt-get install -yqq libxml-xpath-perl

# ----------------
# Install Node 8 (LTS)
# ----------------
# RUN curl -sL https://deb.nodesource.com/setup_8.x | bash - \ && apt-get -y install nodejs \ && npm install npm@latest -g
# ENV NODE_VERSION=8.11.3
# RUN wget -q -O node-v$NODE_VERSION-linux-x64.tar.xz https://nodejs.org/dist/v$NODE_VERSION/node-v$NODE_VERSION-linux-x64.tar.xz
# RUN tar -xJf node-v$NODE_VERSION-linux-x64.tar.xz -C /usr/local --strip-components=1
# RUN rm node-v$NODE_VERSION-linux-x64.tar.xz
# RUN ln -s /usr/local/bin/node /usr/local/bin/nodejs
# ----------------
# Install Chrome ... and add about 600 MB to our image :-(
# ----------------
# RUN wget -q -O - https://dl.google.com/linux/linux_signing_key.pub | apt-key add -
# RUN echo "deb https://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google-chrome.list
# RUN apt-get update -yq
# RUN apt-get -yqq install google-chrome-stable

# ARG CHROME_VERSION="google-chrome-stable" 
# RUN wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add - \ && echo "deb http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google-chrome.list \ && apt-get update -qqy \ && apt-get -y install ${CHROME_VERSION:-google-chrome-stable} \ && rm /etc/apt/sources.list.d/google-chrome.list \ && rm -rf /var/lib/apt/lists/ /var/cache/apt/

# RUN apt-get install -y --fix-missing python-pip \ && pip install --upgrade pip \ && pip install awscli \ && mkdir ~/.aws
# ----------------
# Set up our nodejs packages to allow our Nightwatch E2E tests to be run from the GitLab CI job.
# ----------------
## Install dependencies: nodejs, chrome and xvfb
RUN \
curl -sL https://deb.nodesource.com/setup_6.x | bash - && \
wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add - && \
echo "deb http://dl.google.com/linux/chrome/deb/ stable main" > /etc/apt/sources.list.d/google.list && \
apt-get update && \
apt-get install -y nodejs google-chrome-stable xvfb && \
npm install -g nightwatch && \
rm -rf /var/lib/apt/lists/*

WORKDIR /tests

## Fix for selenium issue, see: https://github.com/SeleniumHQ/docker-selenium/issues/87
ENV DBUS_SESSION_BUS_ADDRESS=/dev/null

## Run nightwatch tests using xvfb
CMD xvfb-run --server-args="-screen 0 1600x1200x24" nightwatch
# The end