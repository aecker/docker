# CUDA on Tesla K40.
#
# Alexander Ecker
# 12/9/2015

FROM bethgelab-users
MAINTAINER Alexander Ecker "alexander.ecker@uni-tuebingen.de"

RUN apt-get install wget

RUN cd /tmp && \
    wget http://developer.download.nvidia.com/compute/cuda/7_0/Prod/local_installers/cuda_7.0.28_linux.run && \
    chmod +x cuda_*_linux.run && ./cuda_*_linux.run -extract=`pwd` && \
    ./NVIDIA-Linux-x86_64-*.run -s --no-kernel-module && \
    ./cuda-linux64-rel-*.run -noprompt && \
    rm -rf *
  
# Add cudnn3 (cudnn needs to be downloaded)
ADD cudnn-7.0-linux-x64-v3.0-prod.tgz /tmp/
RUN cd /tmp && \
    cp -r cuda/include/* /usr/local/cuda/include/ && \
    cp -r cuda/lib64/* /usr/local/cuda-7.0/lib64/ && \
    cp -r cuda/lib64/* /usr/local/cuda/lib64/ && \
    rm -rf *

# Add to path
ENV PATH=/usr/local/cuda/bin:$PATH \
    LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH
