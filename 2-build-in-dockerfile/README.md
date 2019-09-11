# Build In Dockerfile

With this Dockerfile you dont need to compile the app on your machine. The Dockerfile builds and runs the app.

## How to Run

Here you just need to build the docker image.
```sh
docker build -t buildIn-go .
```
and run it
```sh
docker run --rm -it buildIn-go
```

## Improvements

Now you have the advantage that pretty much every one can use you Docker image, because the app isnt compiled on your machine.

## Problems

There are two problems with this approach.

The first problem is about the seperation of concerns.
Here the app is build and run in the same container which brings a big problem.<br>Security.
 
We have a full Linux Distribution behind the base image. Which is good for development but bad for production, because there are hundreds of programms installed which all could have security vulnerabilities.

The second problem is the size of the base image. To be exact 810MB
```sh
REPOSITORY                           TAG                    IMAGE ID            CREATED             SIZE
go-test                              latest                 4ae17f9795f5        6 minutes ago       810MB
``` 
__810MB__ for one simple app is way to large. You can use a `alpine` version of the base image.
```sh
REPOSITORY                           TAG                    IMAGE ID            CREATED             SIZE
go-test                              latest                 e67e11ca07b2        9 seconds ago       367MB
```
That reduces the image size down to 367MB. Thats not bad, but it can be smaller. To achive that you can use multistage Dockerfiles. See the <a href="../3-multistage/README.md">next chapter</a>.
