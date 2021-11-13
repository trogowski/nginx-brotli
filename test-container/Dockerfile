FROM trogowski/nginx-brotli:1.21.4.1

RUN rm /etc/nginx/conf.d/*

COPY nginx-server.conf /etc/nginx/conf.d/nginx-server.conf
COPY public /usr/share/nginx/html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
