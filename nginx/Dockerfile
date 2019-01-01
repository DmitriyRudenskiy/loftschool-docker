FROM nginx:1.15.4-alpine
MAINTAINER Dmitriy Rudenskiy <dmitriy.rudenskiy@gmail.com>
COPY ./nginx.conf /etc/nginx/conf.d/default.conf

RUN ln -sf /dev/stdout /var/log/nginx/access.log \
    	&& ln -sf /dev/stderr /var/log/nginx/error.log