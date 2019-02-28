#
# This Dockerfile builds Softhsmv2 inside Alpine Image
#

ARG    alpine_version=3.8

# Image to be used for Building
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

# Final Image

FROM   alpine:${alpine_version}

ARG    alpine_version=${alpine_version}

COPY   --from=builder /usr/bin/softhsm2-* /usr/bin/
COPY   --from=builder /etc/softhsm2.conf /etc/softhsm2.conf
COPY   --from=builder /usr/lib/softhsm/libsofthsm2.so /usr/lib/softhsm/libsofthsm2.so

ENV    ALPINE_VERSION=${alpine_version}

RUN    apk add --no-cache musl-dev libgcc libstdc++ zlib libcrypto1.0 && \
       mkdir -p /var/lib/softhsm/tokens/
