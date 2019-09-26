# Golang Dockerfiles

> I wrote a blog article about this topic see [here](https://medium.com/@weis98max/golang-multistage-dockerfile-d58c57edeae7)

This project containes different ways you can build your Golang Docker images.

- 1-Simple uses local compiled go binary.
- 2-build.in.docker builds the go binaries inside the containr
- 3-multistage builds the app in two stages and saves spae :D
- 4-bonus creates a custom base image for recurring layers
