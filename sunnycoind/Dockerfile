FROM ubuntu:18.04

MAINTAINER sunnycoin <info@sunny-inc.com>

# user
RUN useradd -M -s /usr/sbin/nologin sunnycoind

# package
RUN apt-get update
RUN apt-get -y upgrade
RUN apt-get -y install git cmake pkg-config protobuf-compiler libprotobuf-dev libssl-dev wget

# sunnycoind dir
RUN mkdir -p /opt/sun/share/doc/sunnycoind
RUN mkdir -p /opt/sun/etc/ssl
RUN mkdir -p /opt/sun/bin
RUN chown -R sunnycoind:sunnycoind /opt/sun

RUN mkdir -p /etc/opt/sunnycoin
WORKDIR /etc/opt/sunnycoin
RUN ln -s /opt/sun/etc/validators.txt validators.txt
RUN ln -s /opt/sun/etc/sunnycoind.cfg sunnycoind.cfg

# block dir
RUN mkdir -p /var/lib/sunnycoind
RUN chown sunnycoind:sunnycoind /var/lib/sunnycoind

# download sunnycoind and boost_1_67_0
RUN mkdir -p /usr/src/app
WORKDIR /usr/src/app

# boost
RUN wget https://dl.bintray.com/boostorg/release/1.67.0/source/boost_1_67_0.tar.gz
RUN tar xvzf boost_1_67_0.tar.gz

# sunnycoind
ARG GIT_REPOSITORY_URL
RUN git clone ${GIT_REPOSITORY_URL}

ARG HARDWARE_AVAILABLE_CPU_CORES=4

# build boost_1_67_0
WORKDIR /usr/src/app/boost_1_67_0
RUN chmod +x bootstrap.sh
RUN ./bootstrap.sh
RUN ./b2 -j ${HARDWARE_AVAILABLE_CPU_CORES}
ENV BOOST_ROOT /usr/src/app/boost_1_67_0

# build sun
RUN mkdir -p /usr/src/app/SUN/my_build
WORKDIR /usr/src/app/SUN/my_build
RUN cmake ..
RUN cmake --build . -- -j ${HARDWARE_AVAILABLE_CPU_CORES}

RUN mv sunnycoind /opt/sun/bin
RUN ln -s /opt/sun/bin/sunnycoind /usr/bin/sunnycoind
ENV PATH $PATH:/opt/sun/bin/

RUN chown -R sunnycoind:sunnycoind /opt/sun

# cleanup
RUN rm -rf /usr/src/app/*

WORKDIR /opt/sun/bin

CMD ["/opt/sun/bin/sunnycoind", "--conf", "/opt/sun/etc/sunnycoind.cfg"]

EXPOSE 5005 51135
