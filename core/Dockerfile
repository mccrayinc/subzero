# USAGE:
#   docker build -t subzero .
#   docker run -t -p 32366:32366 --name subzero subzero

FROM centos:7

RUN yum -y install \
    autoconf \
    automake \
    clang \
    cmake \
    gcc \
    gcc-c++ \
    git \
    libtool \
    make \
    python-setuptools \
    unzip \
    vim

# Protobuf support: 3.5.2
RUN mkdir protobuf && \
    cd protobuf && \
    curl -sSOL https://github.com/google/protobuf/archive/v3.5.2.zip && \
    unzip v3.5.2.zip && \
    cd protobuf-3.5.2 && \
    ./autogen.sh && \
    ./configure && \
    make install && \
    ldconfig && \
    cd python && python setup.py install

ADD CMakeLists.txt /subzero/CMakeLists.txt
ADD include /subzero/include
ADD nanopb /subzero/nanopb
ADD proto /subzero/proto
ADD src /subzero/src
ADD trezor-crypto /subzero/trezor-crypto

WORKDIR /subzero

RUN mkdir /build && cd /build && TARGET=dev CURRENCY=btc-testnet cmake /subzero && make

EXPOSE 32366

CMD ["/build/subzero"]
