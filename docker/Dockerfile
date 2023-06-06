# 
# stage:speedtest
# 
FROM --platform=$TARGETPLATFORM debian:bullseye as speedtest

# Install basics
RUN apt-get update && apt-get install -y curl

# Install speedtest cli
RUN curl -s https://packagecloud.io/install/repositories/ookla/speedtest-cli/script.deb.sh | bash
RUN apt-get install speedtest

# 
# stage:golang
# 
FROM golang:1.20.4-alpine as exporter

ARG TARGETARCH
ARG TARGETVARIANT

WORKDIR /app

ENV GOOS=linux
ENV CGO_ENABLED=0
ENV GOARCH=$TARGETARCH

# COPY go.mod and go.sum files to the workspace
COPY go.mod . 
COPY go.sum .

# Get dependancies - will be cached if mod/sum is not changed
RUN go mod download

# Copy the source code as the last step
COPY . .
RUN go build -o /speedtest-prometheus-exporter .

# 
# stage:app
# 
FROM --platform=$TARGETPLATFORM gcr.io/distroless/static:nonroot

COPY --from=speedtest /usr/bin/speedtest /usr/bin/speedtest
COPY --from=exporter /speedtest-prometheus-exporter /speedtest-prometheus-exporter

CMD [ "/speedtest-prometheus-exporter" ]