FROM ubuntu:20.04

RUN apt clean   &&      \
    apt-get update &&   \
    apt-get install -y keyboard-configuration

ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update                      \
    && apt-get install -y               \
           build-essential              \
           cmake                        \
           git                          \
           lsb-release                  \
           wget                         \
           software-properties-common   \
           sudo                         \
    && wget -O - https://apt.llvm.org/llvm-snapshot.gpg.key | sudo apt-key add -                                \
    && add-apt-repository "deb http://apt.llvm.org/focal/     llvm-toolchain-focal-14    main"                  \
    && apt-get update                                                                                           \
    && apt-get install -y libclang-14-dev                                                                       \
    && rm -rf /var/lib/apt/lists/*

RUN adduser ardupilot             \
    && usermod -aG sudo ardupilot \
    && echo "ardupilot ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers
WORKDIR /home/ardupilot/
USER ardupilot
ENV USER ardupilot

# TODO: we need to merge backup to master
RUN git clone https://github.com/obicons/sa4u   \
    && cd sa4u/src                              \
    && git submodule update --init --recursive  \
    && make                                     \
    && sudo cp sa4u /usr/bin/sa4u               \
    && sudo chmod a+rx /usr/bin/sa4u

ENTRYPOINT ["/usr/bin/sa4u"]
