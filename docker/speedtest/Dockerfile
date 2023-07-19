FROM debian:10-slim AS get-speedtest

RUN apt-get update && apt-get install gnupg1 apt-transport-https dirmngr lsb-release -y
RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 379CE192D401AB61
RUN echo "deb https://ookla.bintray.com/debian $(lsb_release -sc) main" | tee  /etc/apt/sources.list.d/speedtest.list
RUN apt-get update
RUN apt-get install speedtest

FROM node:16.13-alpine as prod-stage
LABEL maintainer="contact@sjultra.com"

COPY --from=get-speedtest /usr/bin/speedtest /usr/bin/speedtest

WORKDIR /app

# install depencies
COPY package.json package-lock.json ./
RUN npm ci

# run script
COPY . .
CMD [ "node", "index.js" ]

