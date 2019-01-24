FROM jenkins/jenkins:lts

USER root

###############################
# Installing DOCKER binaries  #
###############################
RUN apt-get update && \
    apt-get -y install apt-transport-https \
         ca-certificates \
         curl \
         gnupg2 \
         software-properties-common && \
    curl -fsSL https://download.docker.com/linux/$(. /etc/os-release; echo "$ID")/gpg > /tmp/dkey; apt-key add /tmp/dkey && \
    add-apt-repository \
       "deb [arch=amd64] https://download.docker.com/linux/$(. /etc/os-release; echo "$ID") \
       $(lsb_release -cs) \
       stable" && \
    apt-get update && \
    apt-get -y install docker-ce

RUN docker --version


################################
# Installing SINGULARITY 3.0.1 #
################################

# Basic dependencies
RUN apt-get update && apt-get install -y \
    build-essential \
    libssl-dev \
    uuid-dev \
    libgpgme11-dev \
    squashfs-tools \
    libseccomp-dev \
    pkg-config

# Installing GO
RUN export VERSION=1.11.2 OS=linux ARCH=amd64 && \
    wget https://dl.google.com/go/go$VERSION.$OS-$ARCH.tar.gz && \
    tar -C /usr/local -xzvf go$VERSION.$OS-$ARCH.tar.gz && \
    rm go$VERSION.$OS-$ARCH.tar.gz

RUN echo 'export GOPATH=${HOME}/go' >> /root/.bashrc && \
    echo 'export PATH=/usr/local/go/bin:${PATH}:${GOPATH}/bin' >> /root/.bashrc

ENV GOPATH ${HOME}/go
ENV PATH /usr/local/go/bin:${PATH}:${GOPATH}/bin

RUN go get -u github.com/golang/dep/cmd/dep

# Download source
RUN go get -d github.com/sylabs/singularity; exit 0

ENV VERSION v3.0.1
RUN cd $GOPATH/src/github.com/sylabs/singularity && \
    git fetch && \
    git checkout $VERSION; exit 0

# Building Singularity
RUN cd $GOPATH/src/github.com/sylabs/singularity && \
    ./mconfig --prefix=/opt/singularity && \
    make -C ./builddir && \
    make -C ./builddir install


RUN echo 'export PATH=/opt/singularity/bin:${PATH}' >> /root/.bashrc

env PATH /opt/singularity/bin:${PATH}

RUN /opt/singularity/bin/singularity --version
