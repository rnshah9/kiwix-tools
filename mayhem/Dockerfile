FROM --platform=linux/amd64 ubuntu:22.04 as builder

RUN apt-get update
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y build-essential meson pkg-config libzim-dev git libicu-dev libpugixml-dev wget libcurl4-openssl-dev libmicrohttpd-dev libz-dev aria2

RUN wget https://raw.githubusercontent.com/kainjow/Mustache/master/mustache.hpp
RUN mv mustache.hpp /usr/include
RUN git clone https://github.com/kiwix/libkiwix
WORKDIR  /libkiwix/build
RUN meson ..
RUN ninja install

COPY . /repo
WORKDIR /repo/build
RUN meson .. --prefix=/install
RUN ninja install

FROM ubuntu:22.04 as package
RUN apt-get update
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y libzim-dev libpugixml-dev libcurl4-openssl-dev libmicrohttpd-dev
COPY --from=builder /repo/build/src/manager/kiwix-manage /
COPY --from=builder /libkiwix/build/src /deps/
ENV LD_LIBRARY_PATH=/deps
