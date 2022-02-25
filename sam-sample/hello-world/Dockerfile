FROM alpine:latest as build
# install build tools
RUN apk add go git
RUN go env -w GOPROXY=direct
# cache dependencies
ADD go.mod go.sum ./
RUN go mod download
# build
ADD / /
RUN go build -o /main main.go

# copy artifacts to a clean image
FROM alpine:latest
COPY --from=build /main /main
ENTRYPOINT [ "/main" ]