FROM debian:10-slim AS get-speedtest

RUN apt-get update
RUN apt-get install curl -y
RUN curl -s https://packagecloud.io/install/repositories/ookla/speedtest-cli/script.deb.sh | bash
RUN apt-get install speedtest

FROM node:16.13-alpine AS prod-stage
LABEL maintainer="contact@sjultra.com"

WORKDIR /app

# install depencies
COPY docker/package.json docker/package-lock.json .
RUN npm ci

# include speedtest cli in build
COPY --from=get-speedtest /usr/bin/speedtest /usr/bin/speedtest

# run script
COPY docker/index.js .
CMD [ "node", "index.js" ]
