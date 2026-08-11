FROM golang:1.24-alpine AS builder

WORKDIR /app

COPY go.mod ./
RUN go mod download

COPY . .

RUN go build -o go-web-app .

FROM alpine:latest

WORKDIR /app

COPY --from=builder /app/go-web-app .
COPY --from=builder /app/static ./static

EXPOSE 8080

CMD ["./go-web-app"]
