FROM ubuntu:16.04
# add custom china source
ADD sources.list /etc/apt/
RUN apt-get update && apt-get install -y openssh-server
# add account
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
ENV NODE_VERSION 8.9.1

# install nvm
# https://github.com/creationix/nvm#install-script
RUN curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.31.2/install.sh | bash
# nvm change source
RUN export NVM_NODEJS_ORG_MIRROR=https://npm.taobao.org/mirrors/node/

# install node and npm and cnpm
RUN source $NVM_DIR/nvm.sh \
    && NVM_NODEJS_ORG_MIRROR=https://npm.taobao.org/mirrors/node/ nvm install $NODE_VERSION \
    && nvm alias default $NODE_VERSION \
    && nvm use default
#add node and npm to path so the commands are available
ENV NODE_PATH $NVM_DIR/v$NODE_VERSION/lib/node_modules
ENV PATH $NVM_DIR/versions/node/v$NODE_VERSION/bin:$PATH

#npm change source
RUN npm config set registry https://registry.npm.taobao.org

# install common tool,insatll java, python for node_modules
RUN apt-get update && apt-get install -y iputils-ping tmux vim redis-server rsync nginx \
    python python-dev python-pip python-virtualenv

# init tmux setting （personal habbit）
#ADD .tmux.conf ~/.tmux.conf
#RUN tmux source ~/.tmux.conf
ADD socket.conf /etc/nginx/conf.d/
ADD nginx.conf /tmp/
RUN cat /tmp/nginx.conf > /etc/nginx/nginx.conf

#install npm common tool
RUN npm install vue-cli -g
RUN npm install -g cnpm --registry=https://registry.npm.taobao.org
RUN npm install -g forever

EXPOSE 22 3000 3001 3002 3003 3004 3005 3006 3007 3008 3009 3010
CMD ["/usr/sbin/sshd", "-D"]
