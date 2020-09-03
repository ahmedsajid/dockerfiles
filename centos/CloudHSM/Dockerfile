# Docker image with running cloudhsm client
# Pre-req: Have CloudHSM fully setup on the node

FROM centos:7 as base
COPY entrypoint.sh /
RUN yum -y update \
    && yum -y install wget libedit libtool-dev \
    && wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL7/cloudhsm-client-latest.el7.x86_64.rpm \
    && yum -y install cloudhsm-client-latest.el7.x86_64.rpm \
    && yum clean all \
    && rm -rf cloudhsm-client-latest.el7.x86_64.rpm \
    && chmod +x entrypoint.sh
FROM scratch
COPY --from=base / /
CMD ["bash","/entrypoint.sh"]
