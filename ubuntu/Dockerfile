FROM ubuntu:16.04
# add custom china source
ADD sources.list /etc/apt/
RUN apt-get update && apt-get install -y openssh-server
RUN mkdir /var/run/sshd
RUN echo 'root:vagrant' | chpasswd
RUN sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config

# SSH login fix. Otherwise user is kicked off after login
RUN sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd

ENV NOTVISIBLE "in users profile"
RUN echo "export VISIBLE=now" >> /etc/profile

# Install nodejs LTS
RUN rm /bin/sh && ln -s /bin/bash /bin/sh
RUN apt-get install -y curl && apt-get -y autoclean
ENV NVM_DIR /usr/local/nvm
ENV NODE_VERSION 8.1.4

# install nvm
# https://github.com/creationix/nvm#install-script
RUN curl --silent -o- https://raw.githubusercontent.com/creationix/nvm/v0.31.2/install.sh | bash

# install node and npm
RUN source $NVM_DIR/nvm.sh \
    && nvm install $NODE_VERSION \
    && nvm alias default $NODE_VERSION \
    && nvm use default

# confirm installation
RUN node -v
RUN npm -v

# install common tool
RUN apt-get install iputils-ping && apt-get install tmux
# 安装tmux

EXPOSE 22 3000 3001 3002 3003 3004 3005 3006 3007 3008 3009 3010
CMD ["/usr/sbin/sshd", "-D"]
