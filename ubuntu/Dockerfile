FROM ubuntu:16.04
# add custom china source
ADD sources.list /etc/apt/
RUN apt-get update && apt-get install -y openssh-server
RUN apt-get install -yq gconf-service libasound2 libatk1.0-0 libc6 libcairo2 libcups2 libdbus-1-3 \
    libexpat1 libfontconfig1 libgcc1 libgconf-2-4 libgdk-pixbuf2.0-0 libglib2.0-0 libgtk-3-0 libnspr4 \
    libpango-1.0-0 libpangocairo-1.0-0 libstdc++6 libx11-6 libx11-xcb1 libxcb1 libxcomposite1 \
    libxcursor1 libxdamage1 libxext6 libxfixes3 libxi6 libxrandr2 libxrender1 libxss1 libxtst6 \
    ca-certificates fonts-liberation libappindicator1 libnss3 lsb-release xdg-utils wget
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
ENV NODE_VERSION 8.1.4

# install nvm
# https://github.com/creationix/nvm#install-script
RUN curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.31.2/install.sh | bash

# install node and npm and cnpm
RUN source $NVM_DIR/nvm.sh \
    && nvm install $NODE_VERSION \
    && nvm alias default $NODE_VERSION \
    && nvm use default
# add node and npm to path so the commands are available
ENV NODE_PATH $NVM_DIR/v$NODE_VERSION/lib/node_modules
ENV PATH $NVM_DIR/versions/node/v$NODE_VERSION/bin:$PATH
# npm change source
RUN npm config set registry https://registry.npm.taobao.org


# install common tool,insatll java
RUN apt-get install -y iputils-ping \
    && apt-get -y install tmux \
    && apt-get -y install vim \
    && apt-get install -y default-jre \
    && apt-get install -y default-jdk \
    && apt-get install -y unzip


#install npm common tool
RUN npm install vue-cli -g
RUN npm install -g cnpm --registry=https://registry.npm.taobao.org
#RUN mkdir /tmp/chrome-
RUN npm install -g forever
EXPOSE 22 3000 3001 3002 3003 3004 3005 3006 3007 3008 3009 3010
CMD ["/usr/sbin/sshd", "-D"]
