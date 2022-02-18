FROM alpine:latest as build
# install build tools
RUN apk add go git
RUN go env -w GOPROXY=direct
# cache dependencies
ADD go.mod go.sum ./
RUN go mod download
# build
ADD cmd/ cmd/
RUN go build -o /main cmd/app2/main.go

# copy artifacts to a clean image
FROM alpine:latest
COPY --from=build /main /main
ENTRYPOINT [ "/main" ]