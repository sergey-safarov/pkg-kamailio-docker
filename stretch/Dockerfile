FROM debian/eol:stretch
LABEL org.opencontainers.image.authors="Victor Seva <linuxmaniac@torreviejawireless.org>"

# Important! Update this no-op ENV variable when this Dockerfile
# is updated with the current date. It will force refresh of all
# of the base images and things like 'apt-get update' won't be using
# old cached versions when the Dockerfile is built.
ENV REFRESHED_AT="2025-01-23"

# fix repositories
RUN sed -i -e 's/deb.debian.org/archive.debian.org/g' -e '/security.debian.org/d' -e '/stretch-updates/d' /etc/apt/sources.list
RUN rm -rf /var/lib/apt/lists/* && apt-get update
RUN echo MIRRORSITE=http://archive.debian.org/debian > /etc/pbuilderrc
RUN DEBIAN_FRONTEND=noninteractive apt-get install -qq --assume-yes  clang-3.8 pbuilder cmake

VOLUME /code

RUN mkdir -p /usr/local/src/pkg
COPY debian /usr/local/src/pkg/debian

# get build dependences
RUN cd /usr/local/src/pkg/ && /usr/lib/pbuilder/pbuilder-satisfydepends-experimental

# clean
RUN apt-get clean && rm -rf /var/lib/apt/lists/*
WORKDIR /code
