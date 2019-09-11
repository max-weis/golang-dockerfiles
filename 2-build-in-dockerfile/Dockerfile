FROM golang:1.13

WORKDIR $GOPATH/src/app

COPY . .

RUN go mod download

RUN go build main.go

CMD [ "./main" ]
