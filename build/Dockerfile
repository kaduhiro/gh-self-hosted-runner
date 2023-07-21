ARG TIMEZONE=Asia/Tokyo
ARG OS_VERSION=22.04

FROM ubuntu:$OS_VERSION
LABEL maintainer="kaduhiro <github@kaduhiro.com>"

RUN apt-get update \
    && apt-get upgrade -y

RUN DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends tzdata \
    && ln -fns /usr/share/zoneinfo/$TIMEZONE /etc/localtime

RUN apt-get install -y --no-install-recommends \
    ca-certificates curl tini make gettext

ARG RUNNER_PATH=/usr/local/actions-runner
ARG RUNNER_OS
ARG RUNNER_ARCH
ARG RUNNER_VERSION

RUN mkdir $RUNNER_PATH
WORKDIR $RUNNER_PATH

RUN curl -fsSLO https://github.com/actions/runner/releases/download/v$RUNNER_VERSION/actions-runner-$RUNNER_OS-$RUNNER_ARCH-$RUNNER_VERSION.tar.gz
RUN tar xzf actions-runner-$RUNNER_OS-$RUNNER_ARCH-$RUNNER_VERSION.tar.gz \
    && ./bin/installdependencies.sh \
    && rm -rf actions-runner-$RUNNER_OS-$RUNNER_ARCH-$RUNNER_VERSION.tar.gz

ARG USERNAME=app
ARG GROUPNAME=app
ARG UID=1000
ARG GID=1000

RUN groupadd -g $GID $GROUPNAME \
    && useradd -M -s /sbin/nologin -u $UID -g $GID $USERNAME

RUN chown -R $USERNAME.$GROUPNAME $RUNNER_PATH

RUN apt-get clean -y \
    && rm -rf /var/lib/apt/lists/*

USER $USERNAME
ENTRYPOINT ["/usr/bin/tini", "--"]
