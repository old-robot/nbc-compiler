FROM ubuntu:24.04 as builder
WORKDIR /nbc
COPY src ./src
COPY tests ./tests
COPY Makefile .
RUN apt update; apt install -y gcc make fpc libusb-dev
RUN make
RUN ./nbc ./tests/bools.nbc

FROM ubuntu:24.04
WORKDIR /nbc
RUN apt update; apt install -y libusb-dev
COPY --from=builder /nbc/nbc /nbc/nbc