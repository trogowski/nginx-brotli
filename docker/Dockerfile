FROM debian:buster-slim AS brotli-module

ENV NGINX_VERSION 1.21.6

# Download and compile brotli modules
RUN apt-get update \
  && apt-get install -y wget git libpcre3 libpcre3-dev zlib1g zlib1g-dev openssl libssl-dev gcc make \
  && wget https://nginx.org/download/nginx-${NGINX_VERSION}.tar.gz && tar zxvf nginx-${NGINX_VERSION}.tar.gz \
  && git clone https://github.com/google/ngx_brotli.git \
  && cd ngx_brotli && git submodule update --init && cd / \
  && cd nginx-${NGINX_VERSION} \
  && ./configure --with-compat --add-dynamic-module=../ngx_brotli \
  && make modules \
  && ls objs/*.so

# Base image - version must be the same as ENV specified above!
FROM nginx:1.21.6

# Add and configure brotli modules
COPY --from=brotli-module /nginx-${NGINX_VERSION}/objs/*.so /etc/nginx/modules/

RUN sed -i '1s;^;load_module modules/ngx_http_brotli_static_module.so\;\n;' /etc/nginx/nginx.conf \
 && sed -i '1s;^;load_module modules/ngx_http_brotli_filter_module.so\;\n;' /etc/nginx/nginx.conf
