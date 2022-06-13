FROM golang:alpine as build
WORKDIR /go/src/app
COPY go.mod .
COPY go.sum .
RUN go mod download
COPY . .
RUN CGO_ENABLED=0 go build -o /main ./cmd/main.go
RUN apk --no-cache add ca-certificates

FROM scratch
COPY --chown=0:0 --from=build /main /
COPY --from=build /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/
USER 65534

VOLUME ["/config"]

ENV CLAIM_MAPPING_FILE="/config/config.json"
EXPOSE 8080

ENTRYPOINT ["/main"]
