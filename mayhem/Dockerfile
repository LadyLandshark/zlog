# Build Stage:
FROM --platform=linux/amd64 ubuntu:20.04 as builder

## Install build dependencies.
RUN apt-get update && apt-get install -y cmake clang

ADD . /
RUN rm -rf build && mkdir build
RUN cd 

## Build Step
WORKDIR build
RUN CC=clang CXX=clang cmake .. && make install
WORKDIR /test/fuzzers
RUN clang -fsanitize=fuzzer -o zlog_fuzzer zlog_init_fuzzer.c -lzlog
WORKDIR /

# Package Stage
FROM --platform=linux/amd64 ubuntu:20.04
COPY --from=builder /test/fuzzers/zlog_fuzzer /
COPY --from=builder /build/lib/*.so* /
