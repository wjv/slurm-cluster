FROM ubuntu:20.04 as base

ARG DEBIAN_FRONTEND=noninteractive

RUN apt-get update -y \
    && apt-get install -y --no-install-recommends \
    libopenmpi-dev \
    munge \
    build-essential \
    python3.9 \
    python3-dev \
    python3-pip \
    slurmd \
    slurm-client \
    sudo

#RUN apt-get install -y --no-install-recommends \
#    git \
#    mariadb-server \
#    vim \
#    wget

RUN pip3 install mpi4py

RUN useradd -m admin -s /usr/bin/bash -d /home/admin \
    && echo "admin:admin" | chpasswd \
    && adduser admin sudo \
    && echo "admin     ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers


FROM base as jupyter

RUN apt-get install -y --no-install-recommends \
    apt-transport-https  \
    ca-certificates \
    curl  \
    dirmngr \
    lsb-release \
    nodejs \
    npm

RUN curl -sL https://deb.nodesource.com/setup_12.x | bash -

RUN npm install -g configurable-http-proxy

RUN pip3 install \
    jupyterlab \
    jupyterlab_slurm

COPY cgroup.conf slurm.conf /etc/slurm-llnl/

EXPOSE 8888

WORKDIR /home/admin

ENV USER admin
ENV SHELL bash

COPY jupyter-entrypoint.sh /etc/slurm-llnl/

ENTRYPOINT ["/etc/slurm-llnl/jupyter-entrypoint.sh"]


FROM base AS master

RUN apt-get install -y --no-install-recommends \
    slurmctld

COPY cgroup.conf slurm.conf /etc/slurm-llnl/

EXPOSE 6817 6818 6819 3306

WORKDIR /home/admin

COPY master-entrypoint.sh /etc/slurm-llnl/

ENTRYPOINT ["/etc/slurm-llnl/master-entrypoint.sh"]


FROM base AS node

COPY cgroup.conf slurm.conf /etc/slurm-llnl/

EXPOSE 6817 6818 6819

WORKDIR /home/admin

COPY node-entrypoint.sh /etc/slurm-llnl/

ENTRYPOINT ["/etc/slurm-llnl/node-entrypoint.sh"]

