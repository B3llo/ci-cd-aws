# Stage 1
FROM node:16-alpine3.14 as build-step

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
WORKDIR /app

# Copy app directory
RUN git clone git@github.com:ME/my_app.git

# Changing app directory
WORKDIR /app/my_app

# Install app dependencies
RUN npm install -g @angular/cli
RUN npm install

# Run Build      
RUN ng build --prod


# Stage 2

FROM nginx:1.20-alpine

# Clean nginx
RUN rm -rf /usr/share/nginx/html/*

# Copy dist
COPY --from=build-step /app/my_app/ /usr/share/nginx/html/

COPY nginx.conf /etc/nginx/nginx.conf

WORKDIR /usr/share/nginx/html

# Permissions
RUN chown root /usr/share/nginx/html/*
RUN chmod 755 /usr/share/nginx/html/*

# Expose port
EXPOSE 4200

# Start
CMD ["nginx", "-g", "daemon off;"]