# Alex' GPU tools
#
# Alexander Ecker
# 12/9/2015

FROM caffe-k40
MAINTAINER Alexander Ecker "alexander.ecker@uni-tuebingen.de"

#install vim
RUN apt-get update && apt-get install -y \
    libedit-dev \
    python-opencv \
    parallel \
    vim

#install datajoint and dependencies
RUN pip install --upgrade pip
RUN pip install PyMySQL pandas networkx
RUN pip install git+https://github.com/datajoint/datajoint-python

#install ffmpeg for making videos from IPNB
RUN apt-get install -y software-properties-common python-software-properties
RUN add-apt-repository ppa:mc3man/trusty-media
RUN apt-get update && apt-get install -y ffmpeg 

#install theano and lasagne
RUN pip2.7 install --upgrade pip
RUN pip2.7 install https://github.com/Lasagne/Lasagne/archive/master.zip

#install tensorflow
RUN pip2.7 install https://storage.googleapis.com/tensorflow/linux/gpu/tensorflow-0.5.0-cp27-none-linux_x86_64.whl

#install python profiling tool
RUN pip2.7 install line_profiler

#install Bokeh (plotting)
RUN pip2.7 install bokeh

# Install LiClipse
RUN cd /tmp && \
    wget https://5d43085bfff97c6ea2c4ee2c7b3af692535ee7ac.googledrive.com/host/0BwwQN8QrgsRpLVlDeHRNemw3S1E/LiClipse%202.4.0/liclipse_2.4.0_linux.gtk.x86_64.tar.gz && \
    cd /usr/local && \
    sudo tar -xf /tmp/liclipse_2.4.0_linux.gtk.x86_64.tar.gz && \
    rm /tmp/liclipse_2.4.0_linux.gtk.x86_64.tar.gz && \
    sudo ln -s `pwd`/liclipse/LiClipse /usr/bin/liclipse

RUN pip2.7 install mpld3

# Add Jupyter notebook to startup script
RUN echo "su -c 'jupyter notebook --ip=0.0.0.0 --no-browser &' $USER" >> /usr/local/bin/startup

WORKDIR $USER_HOME
