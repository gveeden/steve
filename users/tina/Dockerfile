# the base image to Ubuntu
FROM ubuntu:18.04

# Update the repository sources list
RUN apt-get update && apt-get install -y \
    docker.io \
    git \
    openssh-server \
    python-pip \
    curl \
    less \
    sudo \
    unzip \
    vim \
    wget \
    dirmngr \
    software-properties-common \
    libbz2-dev \
    liblzma-dev \
    autoconf \
    automake \
    pkg-config \
    libgtk-3-dev \
    libncurses5-dev

RUN sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9

RUN sudo add-apt-repository 'deb https://cloud.r-project.org/bin/linux/ubuntu bionic-cran35/'

RUN sudo apt-get --fix-broken install -y && sudo apt-get update && sudo apt-get upgrade -y

ENV DEBIAN_FRONTEND=noninteractive

RUN mkdir -p /var/run/sshd  && mkdir -p /root/.bin && mkdir -p /root/install && mkdir -p /root/tina/data

WORKDIR /root/install

#Install kubectl
RUN curl -O https://storage.googleapis.com/kubernetes-release/release/v1.10.1/bin/linux/amd64/kubectl
RUN cp ./kubectl /root/.bin/kubectl && chmod +x /root/.bin/kubectl

# Get plink 1.9
#install plink
RUN wget https://www.cog-genomics.org/static/bin/plink181012/plink_linux_x86_64.zip && unzip plink_linux_x86_64.zip && cp ./plink /root/.bin/plink && chmod +x /root/.bin/plink

#install shapeit
RUN wget https://mathgen.stats.ox.ac.uk/genetics_software/shapeit/shapeit.v2.r837.GLIBCv2.12.Linux.static.tgz && tar -zxvf shapeit.v2.r837.GLIBCv2.12.Linux.static.tgz && cp ./bin/shapeit /root/.bin/shapeit && chmod +x /root/.bin/shapeit

#install KING
RUN wget http://people.virginia.edu/~wc9c/KING/Linux-king.tar.gz && tar -zxvf Linux-king.tar.gz && cp ./king /root/.bin/king && chmod +x /root/.bin/king

#install ADMIXTURE
RUN wget http://software.genetics.ucla.edu/admixture/binaries/admixture_linux-1.3.0.tar.gz && tar -zxvf admixture_linux-1.3.0.tar.gz && cp ./admixture_linux-1.3.0/admixture /root/.bin/admixture && chmod +x /root/.bin/admixture

RUN git clone https://github.com/vcftools/vcftools.git && git clone --recursive https://github.com/vcflib/vcflib.git && wget https://github.com/samtools/samtools/releases/download/1.9/samtools-1.9.tar.bz2 && tar -xvf samtools-1.9.tar.bz2 && git clone https://github.com/lh3/bwa.git

#install vcftools and vcflib
WORKDIR /root/install/vcftools
RUN ./autogen.sh && ./configure && make && make install
WORKDIR /root/install/vcflib
RUN make

#install samtools
WORKDIR /root/install/samtools-1.9
RUN ./configure && make && make install

#install bwa
WORKDIR /root/install/bwa
RUN make

RUN cp /root/install/bwa/bwa /root/.bin/bwa && chmod +x /root/.bin/bwa && cp /root/install/samtools-1.9/samtools /root/.bin/samtools && chmod +x /root/.bin/samtools

WORKDIR /root/tina

COPY bashrc /root/.bashrc

CMD ["/usr/sbin/sshd", "-D"]
