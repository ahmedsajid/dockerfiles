#

ARG    alpine_version=3.8

FROM   alpine:${alpine_version} as builder

ARG    softhsm_version=2.5.0

ENV    SOFTHSM_VERSION=${softhsm_version}

RUN    apk add  build-base sqlite autoconf automake boost-dev libtool p11-kit-dev sqlite-dev cppunit-dev openssl openssl-dev && \
       cd /tmp/ && \
       echo  https://github.com/opendnssec/SoftHSMv2/archive/${SOFTHSM_VERSION}.tar.gz && \
       wget https://github.com/opendnssec/SoftHSMv2/archive/${SOFTHSM_VERSION}.tar.gz && \
       tar -zxvf ${SOFTHSM_VERSION}.tar.gz && \
       cd /tmp/SoftHSMv2-${SOFTHSM_VERSION} && \
       sh autogen.sh && \
       ./configure --prefix=/usr  --sysconfdir=/etc --localstatedir=/var --disable-gost && \
       make -j 16 && \
       make install -j 16

FROM   alpine:${alpine_version}

ARG    alpine_version=${alpine_version}

COPY   --from=builder /usr/bin/softhsm2-* /usr/bin/
COPY   --from=builder /etc/softhsm2.conf /etc/softhsm2.conf
COPY   --from=builder /usr/lib/softhsm/libsofthsm2.so /usr/lib/softhsm/libsofthsm2.so

ENV    ALPINE_VERSION=${alpine_version}

RUN    if [ ${ALPINE_VERSION} > 3.8 ];then export libcrypto_package="libcrypto1.1" ; else export libcrypto_package="libcrypto1.0";fi && \
       apk add --no-cache musl-dev libgcc libstdc++ zlib ${libcrypto_package} && \
       mkdir -p /var/lib/softhsm/tokens/ && \
       softhsm2-util --init-token --slot 0 --label "ForFabric" --so-pin 1234 --pin 98765432
