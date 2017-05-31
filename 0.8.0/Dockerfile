FROM debian:jessie-backports

MAINTAINER Marcelo Almeida <marcelo.almeida@jumia.com>

WORKDIR "/root"

ENV DEBIAN_FRONTEND noninteractive

# INSTALL BUILDER DEPENDENCIES
RUN apt-get update && apt-get install -y --no-install-recommends \
  automake \
  apt-utils \
  build-essential \
  ca-certificates \
  checkinstall \
  cmake \
  libc6-dev \
  libtool \
  libssl-dev \
  lsb-release \
  make \
  re2c \
  runawk \
  wget

COPY src /src

ENV LIBRABBITC_VERSION=0.8.0

RUN mkdir /pkg
RUN apt-get install -y libcurl4-openssl-dev pkg-config && \
  wget --quiet https://github.com/alanxz/rabbitmq-c/releases/download/v${LIBRABBITC_VERSION}/rabbitmq-c-${LIBRABBITC_VERSION}.tar.gz && \
  tar -zxvf rabbitmq-c-${LIBRABBITC_VERSION}.tar.gz && \
  cd rabbitmq-c-${LIBRABBITC_VERSION} && \
  cp -r /src/* . && \
  autoreconf -i && \
  ./configure --prefix=/usr && \
  checkinstall -y --install=no --pkgname='librabbitmq1' --pkgversion='${LIBRABBITC_VERSION}' --pkggroup='libs' --pkgsource='https://github.com/alanxz/rabbitmq-c' --maintainer='Marcelo Almeida \<marcelo.almeida@jumia.com\>' --requires='libc6 \(\>= 2.14\), libssl1.0.0 \(\>= 1.0.0\)' --include=include_etc

#RUN \
#  apt-get install -y libcurl4-openssl-dev pkg-config && \
#  wget https://github.com/alanxz/rabbitmq-c/releases/download/v${LIBRABBITC_VERSION}/rabbitmq-c-${LIBRABBITC_VERSION}.tar.gz && \
#  tar -zxvf rabbitmq-c-${LIBRABBITC_VERSION}.tar.gz && \
#  cd rabbitmq-c-${LIBRABBITC_VERSION} && \
#  mkdir build && cd build && \
#  cmake -DCMAKE_INSTALL_PREFIX=/usr .. && \
#  cmake --build .
#  ./configure --prefix=/usr && \
#  cp -r /src/* . && \
#  checkinstall -y --install=no --pkgname='librabbitmq1' --pkgversion='${LIBRABBITC_VERSION}' --pkggroup='libs' --pkgsource='https://github.com/alanxz/rabbitmq-c' --maintainer='Marcelo Almeida \<marcelo.almeida@jumia.com\>' --requires='libc6 \(\>= 2.14\), libssl1.0.0 \(\>= 1.0.0\)' --include=include_etc

VOLUME ["/pkg"]
