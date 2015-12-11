# A docker container with caffe (caffe.berkeleyvision.org) installed
# in GPU mode.

FROM cuda-k40

# Get dependencies
RUN apt-get update && apt-get install -y \
    libprotobuf-dev \
    libleveldb-dev \
    libsnappy-dev \
    libopencv-dev \
    libboost-all-dev \
    libhdf5-serial-dev \
    protobuf-compiler \
    gcc-4.6 \
    g++-4.6 \
    gcc-4.6-multilib \
    g++-4.6-multilib \
    gfortran \
    libjpeg62 \
    libfreeimage-dev \
    libatlas-base-dev \
    git \
    python-dev \
    python-pip \
    bc \
    wget \
    curl \
    unzip \
    cmake \
    liblmdb-dev \
    pkgconf

# Use gcc 4.6
RUN update-alternatives --install /usr/bin/cc cc /usr/bin/gcc-4.6 30 && \
    update-alternatives --install /usr/bin/c++ c++ /usr/bin/g++-4.6 30 && \
    update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-4.6 30 && \
    update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-4.6 30

# Allow it to find CUDA libs
RUN echo "/usr/local/cuda/lib64" > /etc/ld.so.conf.d/cuda.conf && \
    ldconfig

# Clone the Caffe repo
RUN cd /opt && \
    git clone https://github.com/BVLC/caffe.git

# Glog
RUN cd /opt && wget https://google-glog.googlecode.com/files/glog-0.3.3.tar.gz && \
    tar zxvf glog-0.3.3.tar.gz && \
    cd /opt/glog-0.3.3 && \
    ./configure && \
    make && \
    make install

# Workaround for error loading libglog:
# error while loading shared libraries: libglog.so.0: cannot open shared object file
# The system already has /usr/local/lib listed in /etc/ld.so.conf.d/libc.conf, so
# running `ldconfig` fixes the problem (which is simpler than using $LD_LIBRARY_PATH)
# TODO: looks like this needs to be run _every_ time a new docker instance is run,
# so maybe LD_LIBRARY_PATh is a better approach (or add call to ldconfig in ~/.bashrc)
RUN ldconfig

# Gflags
RUN cd /opt && \
    wget https://github.com/schuhschuh/gflags/archive/master.zip && \
    unzip master.zip && \
    cd /opt/gflags-master && \
    mkdir build && \
    cd /opt/gflags-master/build && \
    export CXXFLAGS="-fPIC" && \
    cmake .. && \
    make VERBOSE=1 && \
    make && \
    make install

# Build Caffe core
RUN cd /opt/caffe && \
    cp Makefile.config.example Makefile.config && \
    echo "USE_CUDNN := 1" >> Makefile.config && \
    echo "WITH_PYTHON_LAYER := 1" >> Makefile.config && \
    echo "CXX := /usr/bin/g++-4.6" >> Makefile.config && \
    echo "BLAS := open" >> Makefile.config && \
    echo "BLAS_INCLUDE := /opt/OpenBLAS/include" >> Makefile.config && \
    echo "BLAS_LIB := /opt/OpenBLAS/lib" >> Makefile.config && \
    sed -i 's/CXX :=/CXX ?=/' Makefile && \
    make all -j 10

# Install python deps
RUN python2 -m pip install scikit-image scikit-learn h5py leveldb networkx python-dateutil protobuf python-gflags pyyaml pillow

# Build Caffe python bindings and make + run tests
RUN cd /opt/caffe && \
    make pycaffe

# Add binaries to path
RUN bash -c 'echo "export PATH=/opt/caffe/.build_release/tools:\$PATH" >> ~/.bashrc'

ENV PYTHONPATH /opt/caffe/python:$PYTHONPATH
