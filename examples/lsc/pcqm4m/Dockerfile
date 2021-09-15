FROM ubuntu:20.04

RUN apt update
RUN apt -y upgrade
RUN apt install -y sudo
RUN apt install -y tzdata
ARG DEBIAN_FRONTEND=noninteractive
# set your timezone
ENV TZ Asia/Tokyo
RUN echo "${TZ}" > /etc/timezone \
  && rm /etc/localtime \
  && ln -s /usr/share/zoneinfo/Asia/Tokyo /etc/localtime \
  && dpkg-reconfigure -f noninteractive tzdata

RUN apt install -y build-essential
RUN apt install -y git wget ccache time
RUN apt install -y ng-common ng-cjk emacs-nox
RUN apt install -y software-properties-common
RUN wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/cuda-ubuntu2004.pin
RUN mv cuda-ubuntu2004.pin /etc/apt/preferences.d/cuda-repository-pin-600
RUN apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/7fa2af80.pub
RUN add-apt-repository "deb https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/ /"
RUN apt-get update
RUN apt-get -y install cuda

ARG DOCKER_UID=1000
ARG DOCKER_USER=docker
ARG DOCKER_PASSWORD=docker
RUN useradd -u $DOCKER_UID -m $DOCKER_USER --shell /bin/bash && echo "$DOCKER_USER:$DOCKER_PASSWORD" | chpasswd && echo "$DOCKER_USER ALL=(ALL) ALL" >> /etc/sudoers
USER ${DOCKER_USER}

SHELL ["/bin/bash", "-c"]
RUN cd /home/$DOCKER_USER && wget https://repo.anaconda.com/archive/Anaconda3-2021.05-Linux-x86_64.sh
RUN cd /home/$DOCKER_USER && bash ~/Anaconda3-2021.05-Linux-x86_64.sh -b
RUN cd /home/$DOCKER_USER && echo 'export PATH="/home/docker/anaconda3/bin:$PATH"' >> .bashrc 
RUN cd /home/$DOCKER_USER && echo 'export PYTHONPATH="/home/docker/anaconda3/lib/python3.8/site-packages:$PYTHONPATH"' >> .bashrc 
ENV PATH "/home/docker/anaconda3/bin:$PATH"
ENV PYTHONPATH "/home/docker/anaconda3/lib/python3.8/site-packages:$PYTHONPATH"
RUN cd /home/$DOCKER_USER && conda update --all
RUN cd /home/$DOCKER_USER && conda install rdkit -c rdkit -c conda-forge
RUN cd /home/$DOCKER_USER && conda install pyg -c pyg -c conda-forge
RUN cd /home/$DOCKER_USER && conda install pytorch torchvision torchaudio cudatoolkit=11.1 -c pytorch -c nvidia
RUN cd /home/$DOCKER_USER && conda install -c conda-forge tensorboard 
RUN cd /home/$DOCKER_USER && pip install ogb
