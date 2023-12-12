# Getting started
![](https://github.com/rfyiamcool/golang_logo/blob/master/gif/golang_jump.gif)
## Build your Go image
The complete source code for the application is on GitHub: [github.com/anhhieu21/grpc-friendzone](https://github.com/anhhieu21/grpc-friendzone). You are encouraged to fork it and experiment with it as much as you like.
> [!NOTE]
> You need to set up PostgreSQL and other necessary components to run the above example,
> you can refer here : [Set-up PostgreSQL](github.com/anhhieu21/document/blob/main/postgres-debian-server/docs.md).

### Create a Dockerfile for the application
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
### Build the image
The build command optionally takes a `--tag` flag. This flag is used to label the image with a string value, which is easy for humans to read and recognise. If you don't pass a `--tag`, Docker will use `latest` as the default value.
```
docker build --tag grpc-friendzone . 
```
### View local images
To list images, run the `docker image ls` command (or the `docker images` shorthand):

![](/images/docker_image_ls.png)

## Run your Go image as a container
```
docker run -d -p 8080:8080 --name friend-app grpc-friendzone
```
### List container
Since you ran your container in the background, how do you know if your container is running or what other containers are running on your machine? Well, to see a list of containers running on your machine, run docker ps. This is similar to how the ps command is used to see a list of processes on a Linux machine.
```
docker ps
```
### Now, we can testing
Here, i use Postman desktop to testing my proto, download [postman](https://www.postman.com/downloads/)
1. Select the New button and choose gRPC Request.
2. Enter the gRPC server's hostname and port in the server URL. Don't include the http or https scheme in the URL. If the server uses [Transport Layer Security (TLS)](datatracker.ietf.org/doc/html/rfc5246), select the padlock next to the server URL to enable TLS in Postman.
3. Navigate to the Service definition section, then select server reflection or import the app's proto file. When complete, the dropdown list next to the server URL textbox has a list of gRPC methods available.
4. To call a gRPC method, select it in the dropdown, select Generate Example Message, then select Invoke to send the gRPC call to the server.

![](/images/testing_proto.png)

> [!TIP]
> Additionally you can also use gRPCui.