FROM golang:1.21-alpine AS builder

WORKDIR /app

COPY go.mod go.sum ./

RUN apk add --no-cache git && \
    git config --global http.sslVerify false
ENV GOPROXY=direct
ENV GONOSUMDB=*
ENV GOINSECURE=*

RUN go mod download

COPY . .
RUN CGO_ENABLED=0 GOOS=linux go build -o main .

FROM alpine:latest

WORKDIR /app

COPY --from=builder /app/main .

EXPOSE 8081

CMD ["./main"]
