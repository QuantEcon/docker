FROM ubuntu:18.04
MAINTAINER me@arnavsood.com

# Install sudo utility
RUN apt-get update && \
      apt-get -y install sudo

# Set up a user with sudo permissions and switch over
RUN useradd -rmu 999 -g sudo quantecon && \ 
        echo "quantecon ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers
USER quantecon

# Common utilities 
RUN sudo apt-get update && sudo apt-get install --no-install-recommends -y git \ 
    curl \ 
    ca-certificates \ 
    make && \
    sudo apt-get clean
    
# Install Anaconda3 Python (for Jupinx, etc.)
RUN mkdir -p ~/utils/conda && mkdir -p /tmp && cd /tmp && \ 
    curl -O https://repo.anaconda.com/miniconda/Miniconda3-4.5.11-Linux-x86_64.sh && \
    echo "ea4594241e13a2671c5b158b3b813f0794fe58d514795fbf72a1aad24db918cf  Miniconda3-4.5.11-Linux-x86_64.sh" | sha256sum -c - && \
    bash Miniconda3-4.5.11-Linux-x86_64.sh -u -b -p ~/utils/conda && \
    ln -s ~/utils/conda/etc/profile.d/conda.sh /etc/profile.d/conda.sh && \
    echo ". /opt/conda/etc/profile.d/conda.sh" >> ~/.bashrc && \
    echo "conda activate base" >> ~/.bashrc 

CMD [ "/bin/bash" ]
