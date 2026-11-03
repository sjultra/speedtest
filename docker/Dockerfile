# Ookla's static (musl) CLI, straight from its release tarball
FROM alpine:3.20 AS get-speedtest
ARG SPEEDTEST_VERSION=1.2.0
RUN wget -qO- "https://install.speedtest.net/app/cli/ookla-speedtest-${SPEEDTEST_VERSION}-linux-x86_64.tgz" | tar -xz -C /usr/bin speedtest

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
