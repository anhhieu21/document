# Getting started
![](https://github.com/rfyiamcool/golang_logo/blob/master/gif/golang_jump.gif)
## Build your Go image
```
# syntax=docker/dockerfile:1
FROM golang:1.21 AS build-stage

# Set destination for COPY
WORKDIR /app

# Download Go modules
COPY go.mod go.sum ./
RUN go mod download

# Copy the source code. Note the slash at the end, as explained in
# https://docs.docker.com/engine/reference/builder/#copy
COPY *.go ./

# Build
RUN CGO_ENABLED=0 GOOS=linux go build -o /grpc-friendzone

# Optional:
# To bind to a TCP port, runtime parameters must be supplied to the docker command.
# But we can document in the Dockerfile what ports
# the application is going to listen on by default.
# https://docs.docker.com/engine/reference/builder/#expose
EXPOSE 8080


# Run
CMD ["/grpc-friendzone"]
```
## Build the image
The build command optionally takes a `--tag` flag. This flag is used to label the image with a string value, which is easy for humans to read and recognise. If you don't pass a `--tag`, Docker will use `latest` as the default value.
```
docker build --tag grpc-friendzone . 
```
## View local images
To list images, run the docker image lscommand (or the docker images shorthand):
<div>
  <img>
</div>
