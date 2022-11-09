FROM alpine:latest AS build

RUN wget https://golang.org/dl/go1.18.8.linux-amd64.tar.gz
RUN tar -C /usr/local -xzvf go1.18.8.linux-amd64.tar.gz 
RUN apk add --no-cache libc6-compat git protobuf
RUN /usr/local/go/bin/go version


RUN git clone https://github.com/aptu/codelabs

RUN /usr/local/go/bin/go install google.golang.org/protobuf/cmd/protoc-gen-go@v1.28
RUN /usr/local/go/bin/go install google.golang.org/grpc/cmd/protoc-gen-go-grpc@v1.2
RUN cd codelabs/micro/logger && /usr/local/go/bin/go mod tidy

RUN export PATH="/root/go/bin:$PATH" && cd codelabs/micro/logger && sh -x ./build.sh
RUN cd codelabs/micro/logger && /usr/local/go/bin/go build

FROM alpine:latest AS clean
RUN apk add --no-cache libc6-compat
COPY --from=build /codelabs/micro/logger/logger /usr/bin/
EXPOSE 7778/tcp
ENTRYPOINT ["/usr/bin/logger"]