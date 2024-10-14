FROM opensuse/tumbleweed
MAINTAINER maintainer@hardenedlinux.org

ENV LANG="en_US.UTF-8" LC_CTYPE="en_US.UTF-8"

RUN zypper --non-interactive update && \
    zypper --non-interactive install --force-resolution \
    pandoc && \
    zypper clean -a && \
    rm -rf /var/log/* /var/tmp/*

RUN groupadd -g 1000 pan
RUN useradd -m -u 1000 -g pan pan

USER pan
WORKDIR /home/pan/
