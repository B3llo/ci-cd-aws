FROM node:16-alpine3.14

# Install ssh client and git
RUN apk add --no-cache git openssh

# add credentials on build
RUN mkdir /root/.ssh/ && chmod 700 /root/.ssh/ && ssh-keyscan github.com > /root/.ssh/known_hosts

ADD my_ssh /root/.ssh/my_ssh

ADD my_ssh.pub /root/.ssh/my_ssh.pub

# SSH Permisssions
RUN chmod 600 /root/.ssh/my_ssh && chmod 600 /root/.ssh/my_ssh.pub

RUN  echo "IdentityFile ~/.ssh/my_ssh" >> /etc/ssh/ssh_config

# Create app directory
WORKDIR /usr/src/app

# Copy app directory
RUN git clone --branch <Repository_Branch> git@github.com:ME/my_server.git

# Changing app directory
WORKDIR /usr/src/app/my_server

# Install app dependencies
RUN npm install
RUN npm install typescript -g

# Run Build      
RUN tsc

EXPOSE 3000

# Bundle app source
CMD ["node", "dist/main.js"]