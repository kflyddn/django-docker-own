# base container image
FROM python:3.6.1-alpine

# set environment variables for system
ENV PYTHONUNBUFFERED=1

# update system packages
RUN apk update && apk upgrade \
 && apk add --no-cache tzdata \
 bash bash-doc bash-completion \
 mysql-devel gcc gcc-devel python-devel \
 && rm -rf /var/cache/apk/*

# Settings for the local user
ENV APP_USER docker
ENV APP_USER_UID 1000
ENV APP_USER_GROUP docker
ENV APP_USER_GROUP_GID 996

# setup working directory within container
RUN mkdir -p /web
WORKDIR /web

# copy requirements.txt for pip install
COPY requirements.txt /web/requirements.txt

# setup python environment, import required modules
RUN pip install --upgrade pip \
 && pip install --no-cache-dir -r requirements.txt

# Create user
RUN addgroup -g ${APP_USER_GROUP_GID} ${APP_USER_GROUP} \
 && adduser -u ${APP_USER_UID} -D -G ${APP_USER_GROUP} ${APP_USER} \
 && chown -R $APP_USER:$APP_USER_GROUP /web

# copy project files into working directory
USER ${APP_USER}
COPY . /web
