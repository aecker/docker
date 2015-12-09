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

# Create groups
RUN addgroup cin
RUN groupmod -g 1011 cin
RUN addgroup bethgelab
RUN groupmod -g 1019 bethgelab

# Add user and generate a random password with 12 characters that includes at least one capital letter and number.
ENV USER=aecker
ENV USER_ID=1684
# Password: aaC4h78nVbvQI
ENV DOCKER_ENCRYPYTED_PASSWORD=aaMPY5OmkIlEI
RUN useradd -m -d /home/$USER -p $DOCKER_ENCRYPYTED_PASSWORD $USER
RUN usermod -u $USER_ID $USER

# Not sure what this was needed for
# sed -Ei "s/adm:x:4:/$USER:x:4:$USER/" /etc/group

RUN adduser $USER sudo
RUN adduser $USER bethgelab
RUN adduser $USER cin

# Set the default shell as bash for docker user.
RUN chsh -s /bin/bash $USER

# Copy the config files into the docker directory
ADD .bethgelab /home/$USER/.bethgelab
#RUN cd /tmp/.bethgelab && \
#sudo -u $USER mkdir /home/$USER/.bethgelab && \
#sudo -u $USER cp -R .[a-z]* [a-z]* /home/$USER/.bethgelab/

# restarts the xdm service
RUN /etc/init.d/xdm restart

# Start the ssh service
CMD /usr/sbin/sshd -D