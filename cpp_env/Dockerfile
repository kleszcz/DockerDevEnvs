# Article on creating docker dev env: https://www.codemag.com/Article/1811021/Docker-for-Developers
FROM ubuntu:latest
LABEL maintainer="Jan Kleszczyński <jankleszcz@gmail.com>"
# Add script for finding distro codename as lsb_release isn't installed
ADD scripts/codename /usr/local/bin/codename
# Tell dpkg we're not interactive
ENV DEBIAN_FRONTEND=noninteractive
# Install basics
RUN apt-get update && \
    apt-get install -y sudo curl git-core gnupg locales wget vim tzdata&& \
    locale-gen en_US.UTF-8 && \
    adduser --quiet --disabled-password --shell /bin/bash --home /home/devuser --gecos "Dev User" devuser && \
    echo "devuser:devuser" | chpasswd && \
    usermod -aG sudo devuser && \
    # Set codename script as executable
    chmod a+x /usr/local/bin/codename && \
    # Set timezone
    ln -fs /usr/hare/zoneinfo/Europe/Warsaw /etc/localtime
# Install cmake 3.17.3
RUN mkdir ~/cmake_download && cd ~/cmake_download && \
    wget -q https://github.com/Kitware/CMake/releases/download/v3.17.3/cmake-3.17.3-Linux-x86_64.sh && \
    chmod u+x cmake-3.17.3-Linux-x86_64.sh && \
    ./cmake-3.17.3-Linux-x86_64.sh --skip-license --prefix="/usr/local/" && \
    rm -rf ~/cmake_download
# Install gcc 10
RUN echo "deb http://ppa.launchpad.net/ubuntu-toolchain-r/test/ubuntu $(codename) main" >> /etc/apt/sources.list && \
    echo "deb-src http://ppa.launchpad.net/ubuntu-toolchain-r/test/ubuntu $(codename) main" >> /etc/apt/sources.list && \
    apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 60C317803A41BA51845E371A1E9377A2BA9EF27F && \
    apt-get update && \
    apt-get install -y gcc-10 g++-10
# Alias gcc-10 as gcc and cc
RUN update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-10 10 && \
    update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-10 10 && \
    update-alternatives --install /usr/bin/cc cc /usr/bin/gcc 10 && \
    update-alternatives --install /usr/bin/c++ c++ /usr/bin/g++ 10
# Install make
RUN apt-get -y install make
# Install gdb
RUN apt-get -y install gdb
# TODO:
#   - install vcpkg

# Switch user
USER devuser