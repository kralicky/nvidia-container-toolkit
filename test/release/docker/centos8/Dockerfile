FROM centos:8

RUN yum install -y \
    yum-utils \
    ruby-devel \
    gcc \
    make \
    rpm-build \
    rubygems \
    createrepo

RUN gem install --no-document fpm

# We create and install a dummy docker package since these dependencies are out of
# scope for the tests performed here.
RUN fpm -s empty \
    -t rpm \
    --description "A dummy package for docker-ce_18.06.3.ce-3.el7" \
    -n docker-ce --version 18.06.3.ce-3.el7 \
    -p /tmp/docker.rpm \
    && \
    yum localinstall -y /tmp/docker.rpm \
    && \
    rm -f /tmp/docker.rpm


ARG WORKFLOW=nvidia-docker
ARG TEST_REPO=nvidia.github.io
ENV TEST_REPO ${TEST_REPO}
RUN curl -s -L https://nvidia.github.io/${WORKFLOW}/centos8/nvidia-docker.repo \
    | tee /etc/yum.repos.d/nvidia-docker.repo

COPY entrypoint.sh /

ENTRYPOINT [ "/entrypoint.sh" ]