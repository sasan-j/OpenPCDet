FROM nvidia/cuda:11.6.2-cudnn8-devel-ubuntu20.04

RUN echo 'debconf debconf/frontend select Noninteractive' | debconf-set-selections

# Install basics
RUN apt-get update -y \
    && apt-get install build-essential \
    && apt-get install -y apt-utils git curl ca-certificates bzip2 tree htop wget sudo \
    && apt-get install -y libglib2.0-0 libgl1 libsm6 libxext6 libxrender-dev bmon iotop g++ \
     python3.9 python3.9-dev python3-pip python3.9-distutils python-is-python3

# Install Boost geometry
RUN wget https://jaist.dl.sourceforge.net/project/boost/boost/1.68.0/boost_1_68_0.tar.gz && \
    tar xzvf boost_1_68_0.tar.gz && \
    cp -r ./boost_1_68_0/boost /usr/include && \
    rm -rf ./boost_1_68_0 && \
    rm -rf ./boost_1_68_0.tar.gz 

# Install python
# RUN ln -sv /usr/bin/python3.9 /usr/bin/python
# RUN wget https://bootstrap.pypa.io/get-pip.py && \
# 	python get-pip.py && \
# 	rm get-pip.py

RUN useradd -ms /bin/bash jdgr && usermod -aG sudo jdgr && \
    echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers && \
    echo 'export PATH=/home/jdgr/.local/bin:$PATH' >> /home/jdgr/.bashrc
USER jdgr

WORKDIR /home/jdgr/

# Install python packages
RUN python -m pip --no-cache-dir install --user numpy llvmlite numba 

# Install torch and torchvision
# See https://pytorch.org/ for other options if you use a different version of CUDA
# RUN pip install --user torch==1.6 torchvision==0.7.0 -f https://download.pytorch.org/whl/cu102/torch_stable.html
# RUN conda install pytorch==1.13.1 torchvision==0.14.1 pytorch-cuda=11.6 -c pytorch -c nvidia
RUN python -m pip install --user torch==1.13.1 torchvision==0.14.1  --index-url https://download.pytorch.org/whl/cu116

# Install python packages
RUN python -m pip --no-cache-dir install --user tensorboardX easydict pyyaml scikit-image tqdm \
     SharedArray six pyquaternion opencv-python-headless av2 kornia

# A weired problem that hasn't been solved yet
RUN python -m pip uninstall -y SharedArray && \
    python -m pip --no-cache-dir install --user SharedArray spconv-cu116 ipykernel
