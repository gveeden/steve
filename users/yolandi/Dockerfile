# the base image to Ubuntu
FROM ubuntu:17.10

# Update the repository sources list
RUN apt-get update && apt-get install -y \
    docker.io \
    git \
    openssh-server \
    python-pip \
    r-base
    
RUN apt-get install -y \
    curl \
    less \
    sudo \
    unzip \
    vim \
    wget

RUN mkdir -p /var/run/sshd  && mkdir -p /root/.bin && mkdir -p /root/install && mkdir -p /root/yolandi/data

WORKDIR /root/install

#Install kubectl
RUN curl -O https://storage.googleapis.com/kubernetes-release/release/v1.10.1/bin/linux/amd64/kubectl
RUN cp ./kubectl /root/.bin/kubectl && chmod +x /root/.bin/kubectl

RUN pip install snpflip

# Get plink 1.9
#install plink
RUN wget https://www.cog-genomics.org/static/bin/plink181012/plink_linux_x86_64.zip
RUN unzip plink_linux_x86_64.zip
RUN cp ./plink /root/.bin/plink && chmod +x /root/.bin/plink
#RUN cp ./plink /root/src/plink && chmod +x /root/src/plink

#install shapeit
RUN wget https://mathgen.stats.ox.ac.uk/genetics_software/shapeit/shapeit.v2.r837.GLIBCv2.12.Linux.static.tgz
RUN tar -zxvf shapeit.v2.r837.GLIBCv2.12.Linux.static.tgz
RUN cp ./bin/shapeit /root/.bin/shapeit && chmod +x /root/.bin/shapeit
#RUN cp ./bin/shapeit /root/src/shapeit && chmod +x /root/src/shapeit

#install KING 
RUN wget http://people.virginia.edu/~wc9c/KING/Linux-king.tar.gz
RUN tar -zxvf Linux-king.tar.gz
RUN cp ./king /root/.bin/king && chmod +x /root/.bin/king

#install ADMIXTURE
RUN wget http://www.genetics.ucla.edu/software/admixture/binaries/admixture_linux-1.3.0.tar.gz
RUN tar -zxvf admixture_linux-1.3.0.tar.gz
RUN cp ./admixture_linux-1.3.0/admixture /root/.bin/admixture && chmod +x /root/.bin/admixture

#Install RFMIX
RUN git clone https://github.com/slowkoni/rfmix.git
WORKDIR /root/install/rfmix
RUN autoreconf --force --install
RUN ./configure
RUN make
RUN cp ./rfmix /root/.bin/rfmix && chmod +x /root/.bin/rfmix


WORKDIR /root/yolandi

COPY bashrc /root/.bashrc

CMD ["/usr/sbin/sshd", "-D"]

