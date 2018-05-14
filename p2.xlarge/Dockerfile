#FROM nvidia/cuda:8.0-cudnn6-devel-ubuntu16.04
FROM nvidia/cuda:8.0-cudnn5-devel-ubuntu16.04

#######################################################################
# Basic Setup
#

ENV LANG=C.UTF-8 LC_ALL=C.UTF-8
RUN apt-get update && apt-get -y upgrade

#######################################################################
# Anaconda3
#   from continuumio/anaconda3 
#   https://hub.docker.com/r/continuumio/anaconda3/
#

RUN apt-get update --fix-missing && apt-get install -y wget bzip2 ca-certificates \
    libglib2.0-0 libxext6 libsm6 libxrender1 \
    git mercurial subversion

RUN echo 'export PATH=/opt/conda/bin:$PATH' > /etc/profile.d/conda.sh && \
    wget --quiet https://repo.continuum.io/archive/Anaconda3-4.3.1-Linux-x86_64.sh -O ~/anaconda.sh && \
    /bin/bash ~/anaconda.sh -b -p /opt/conda && \
    rm ~/anaconda.sh

RUN apt-get install -y curl grep sed dpkg && \
    TINI_VERSION=`curl https://github.com/krallin/tini/releases/latest | grep -o "/v.*\"" | sed 's:^..\(.*\).$:\1:'` && \
    curl -L "https://github.com/krallin/tini/releases/download/v${TINI_VERSION}/tini_${TINI_VERSION}.deb" > tini.deb && \
    dpkg -i tini.deb && \
    rm tini.deb && \
    apt-get clean

ENV PATH /opt/conda/bin:$PATH

ENTRYPOINT [ "/usr/bin/tini", "--" ]
CMD [ "/bin/bash" ]

#######################################################################
# Deep Learning basic toolkits
#

RUN conda install pip
RUN conda install nomkl numpy scipy scikit-learn numexpr

RUN apt-get update && \
apt-get install -y libopenblas-dev libopenblas-base && \
apt-get install -y git

# Set CUDA_ROOT
ENV CUDA_ROOT /usr/local/cuda/bin
RUN pip install tensorflow-gpu==1.2.1
RUN pip install keras==2.0.6

RUN ln -s /usr/local/nvidia/lib64/libcuda.so.1 /usr/lib/x86_64-linux-gnu/libcuda.so

#######################################################################
# Mount points for data directories
#

RUN mkdir -p /mnt/test_data
RUN mkdir -p /mnt/training_data
RUN mkdir -p /mnt/output
RUN mkdir -p /mnt/logs
RUN mkdir -p /mnt/temp

########################################################################

RUN echo "umask 0000" >> /root/.bashrc

WORKDIR /root
RUN ["/bin/bash"]
