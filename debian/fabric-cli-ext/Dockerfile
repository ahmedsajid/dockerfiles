FROM golang:1.13 as build
RUN mkdir -p ${GOPATH}/src/github.com/trustbloc \
    && cd ${GOPATH}/src/github.com/trustbloc \
    && git clone https://github.com/trustbloc/fabric-cli-ext.git \
    && cd fabric-cli-ext \
    && make plugins \
    && mkdir -p /opt/fabric-cli-ext \
    && find .build -mindepth 1 -maxdepth 1 -type d -exec cp -rav {} /opt/fabric-cli-ext \; \
    && /opt/fabric-cli-ext/bin/fabric plugin install /opt/fabric-cli-ext/ledgerconfig \
    && /opt/fabric-cli-ext/bin/fabric plugin install /opt/fabric-cli-ext/file \
    && /opt/fabric-cli-ext/bin/fabric plugin install /opt/fabric-cli-ext/extensions

FROM golang:1.13 as base
COPY --from=build /opt/fabric-cli-ext /opt/fabric-cli-ext 
COPY --from=build /root/.fabric /root/.fabric
RUN apt-get update \
    && apt-get install -y jq vim unzip \
    && apt-get -y dist-upgrade \
    && apt-get clean autoclean \
    && apt-get autoremove -y \
    && rm -rf /var/lib/cache /var/lib/log /var/lib/apt/lists/*

FROM scratch
COPY --from=base / /
ENV GOPATH /go
ENV PATH /opt/fabric-cli-ext/bin:/usr/local/go/bin:$GOPATH/bin:$PATH
WORKDIR $GOPATH
