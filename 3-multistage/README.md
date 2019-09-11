# Multistage

In the previous chapter I showed you how to create a Docker image of your Golang app, but the smallest one was 367MB big. With a app size of 7MB that seams a bit too much.

## How it works 
With version 17.05 Docker added the multistage feature.
It means that you can define multiple containers in one Dockerfile.

How does it help with smaller image sizes?

Because Go can be compiled to a binary which can be run without any VM's or other programs you dont need any Linux Distribution. You can use `scratch` as the base image. The size of `scratch` is 0.<br>Thats handy.

## Dockerfile

Our Dockerfile uses two stages. First the build stage, where the app is build. And the second and final stage, where the app is run.

```Dockerfile
# build stage
FROM golang:1.13-alpine as build
```
To keep the _build_ stage small we use alpine as the base image.

```Dockerfile
WORKDIR $GOPATH/app/

RUN apk add git
```

Next the `WORKDIR` is defined. Thats the location of our app inside the container. And to download the dependencies go need git. So we install it too.

```Dockerfile
# copy and download dependencies
COPY go.* .
RUN go mod download
```
Here we use the Go Modules which is the default dependency management in Go since version `1.13`.
The dependencies are copied before the source code to cache the layers. That will help you to safe time during the build.

```Dockerfile
#compile app
COPY . .
RUN go build -o main
```
Now the app is compiled.

```Dockerfile
#resulting app
FROM scratch as final
COPY --from=build go/app/main /app/
WORKDIR /app
ENTRYPOINT [ "./main" ]
```
The last step and the second stage.
Here we use `scratch` as the base image and copy the compiled binary from the first stage.
The `--from=build` indicates that the files that need to be copied are from the build stage.