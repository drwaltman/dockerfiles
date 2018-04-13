# References:
# https://github.com/ContinuumIO/docker-images/blob/master/anaconda3/Dockerfile
# https://github.com/Kaggle/docker-python/blob/master/Dockerfile
# https://github.com/MattKleinsmith/dockerfiles/blob/master/fastai/Dockerfile
# https://github.com/Paperspace/fastai-docker/blob/master/Dockerfile
# https://github.com/ceshine/Dockerfiles/blob/master/cuda/fastai/Dockerfile
# https://github.com/hamelsmu/Docker_Tutorial/blob/master/gpu_tutorial/Dockerfile.gpu
# https://github.com/tensorflow/tensorflow/blob/master/tensorflow/tools/docker/Dockerfile.gpu
# https://github.com/pytorch/pytorch/blob/master/Dockerfile

# Choose CUDA image:
# https://store.docker.com/community/images/nvidia/cuda
# https://github.com/NVIDIA/nvidia-docker/wiki/CUDA
FROM nvidia/cuda:9.0-cudnn7-runtime-ubuntu16.04

# Set language to C.UTF-8 to prevent Linux system from breaking on Python 3*
# http://bugs.python.org/issue19846
ENV LANG C.UTF-8
ENV LC_ALL C.UTF-8

# Install apt-get packages
# OpenCV dependencies: libsm6, libxext6, libxrender-dev
# https://github.com/ray-project/ray/issues/923
RUN apt-get update --fix-missing \
    && apt-get install -y --allow-downgrades --no-install-recommends \
        build-essential \
        curl \
        libsm6 \
        libxext6 \
        libxrender-dev \
        software-properties-common \
    && apt-get -y autoremove \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Install conda with Python 3.6
ENV PYTHON_VERSION 3.6
RUN curl -o ~/miniconda.sh -O https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh \
    && chmod +x ~/miniconda.sh \
    && ~/miniconda.sh -b -p /opt/conda \
    && rm ~/miniconda.sh
ENV PATH /opt/conda/bin:$PATH

# Update conda and install PyTorch and dependencies
# http://pytorch.org/
RUN conda update -n base conda \
    && conda install pytorch torchvision cuda90 -c pytorch

# Upgrade pip and install packages, including TensorFlow
# Cython 0.25.2 chosen to avoid AttributeError issue
# https://github.com/cython/cython/issues/1953
RUN pip --no-cache-dir install --upgrade pip \
    && pip --no-cache-dir install --upgrade \
        Cython==0.25.2 \
        html5lib \
        h5py \
        imageio \
        jsonschema \
        jupyter \
        jupyter_contrib_nbextensions \
        keras \
        keras-resnet \
        keras-tqdm \
        matplotlib \
        nltk \
        opencv-python \
        pandas \
        scikit-image \
        scikit-learn \
        scipy \
        tensorflow-gpu \
    && pip --no-cache-dir install --upgrade \
        wordbatch \
    && jupyter contrib nbextension install --user

# Add CUPTI files for TensorFlow CUDA profiling
ENV LD_LIBRARY_PATH /usr/local/cuda/extras/CUPTI/lib64:$LD_LIBRARY_PATH

# Add location to allow access to custom libraries throughout environment
ENV PYTHONPATH /home/jovyan/work/libraries:$PYTHONPATH
