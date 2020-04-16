FROM node:alpine

# Create app directory
RUN mkdir -p /usr/src/app
WORKDIR /usr/src/app

# Install app dependencies
COPY package.json /usr/src/app/

RUN apk add --update \
    python \
    python-dev \
    py-pip \
    build-base

RUN npm install

# Bundle app source
COPY laravel-echo-server.json /usr/src/app/laravel-echo-server.json

# SSL証明書をコピー
COPY ssl/app-burdock.docker.pem /usr/src/app/ssl/app-burdock.docker.pem
COPY ssl/app-burdock.docker-key.pem /usr/src/app/ssl/app-burdock.docker-key.pem

EXPOSE 3000
CMD [ "npm", "start", "--force" ]
