FROM golang:1.17 as build_env

WORKDIR /app
COPY app.go go.mod ./
RUN go get -d -v && go build -o tester .

FROM golang:1.17 as fortio_build_env

WORKDIR /fortio
# TODO: change to use dapr forked repo
ADD "https://api.github.com/repos/skyao/fortio/branches/v1.24.0-dapr" skipcache
RUN git clone https://github.com/skyao/fortio.git
RUN cd fortio && git checkout v1.24.0-dapr && go build

FROM debian:buster-slim
#RUN apt update
#RUN apt install wget -y
WORKDIR /
COPY --from=build_env /app/tester /
COPY --from=fortio_build_env /fortio/fortio/fortio /usr/local/bin
CMD ["/tester"]
