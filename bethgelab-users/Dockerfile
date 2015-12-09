# This file creates a container that allows to use cin/bethgelab 
# user accounts within the container, writing files with the correct
# userid to the /gpfs01 mount.
#
# Alexander Ecker
# 12/9/2015

FROM scipyserver
MAINTAINER Alexander Ecker "alexander.ecker@uni-tuebingen.de"

# Install X11, SSH and sudo
RUN apt-get update -y
RUN apt-get upgrade -y
RUN apt-get install -y openssh-server pwgen xpra rox-filer xserver-xephyr xdm fluxbox xvfb sudo xterm

# Configuring xdm to allow connections from any IP address and ssh to allow X11 Forwarding. 
RUN sed -i 's/DisplayManager.requestPort/!DisplayManager.requestPort/g' /etc/X11/xdm/xdm-config
RUN sed -i '/#any host/c\*' /etc/X11/xdm/Xaccess
RUN ln -s /usr/bin/Xorg /usr/bin/X
RUN echo X11Forwarding yes >> /etc/ssh/ssh_config

# Fix PAM login issue with sshd
RUN sed -i 's/session    required     pam_loginuid.so/#session    required     pam_loginuid.so/g' /etc/pam.d/sshd

# Set locale (fix the locale warnings)
RUN localedef -v -c -i en_US -f UTF-8 en_US.UTF-8 || :

# Expose SSH port
EXPOSE 22

# Create the directory needed to run the sshd daemon
RUN mkdir /var/run/sshd 

# restarts the xdm service
RUN /etc/init.d/xdm restart

# Create groups
RUN addgroup cin
RUN groupmod -g 1011 cin
RUN addgroup bethgelab
RUN groupmod -g 1019 bethgelab

# Add user and generate a random password with 12 characters that includes at least one capital letter and number.
ENV USER=aecker
ENV USER_ID=1684
ENV USER_HOME=/gpfs01/bethge/home/$USER
ENV USER_ENCRYPYTED_PASSWORD=aa5bW7Im4bDqU
RUN useradd -M -d $USER_HOME -p $USER_ENCRYPYTED_PASSWORD $USER
RUN usermod -u $USER_ID $USER

# Add user to groups
RUN adduser $USER sudo
RUN adduser $USER bethgelab
RUN adduser $USER cin

# Set the default shell as bash for docker user.
RUN chsh -s /bin/bash $USER

# Copy the config files into the docker directory
COPY bin/* /usr/local/bin/

# Start the ssh service
CMD /usr/sbin/sshd -D
