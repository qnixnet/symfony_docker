ARG NGINX_VERSION

FROM nginx:${NGINX_VERSION}

# Configure Nginx
COPY nginx.conf /etc/nginx/nginx.conf
COPY symfony.conf /etc/nginx/conf.d/default.conf

ARG SERVER_NAME

RUN sed "/server_name symfony.dev;/c\    server_name ${SERVER_NAME} www.${SERVER_NAME};" -i /etc/nginx/conf.d/default.conf \
    && sed "/proxy_pass http:\/\/symfony.dev;/c\        proxy_pass http:\/\/${SERVER_NAME};" -i /etc/nginx/conf.d/default.conf

WORKDIR /etc/nginx

# Expose ports
EXPOSE 80 443