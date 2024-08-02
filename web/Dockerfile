FROM webdevops/php-nginx-dev:8.1

RUN apt-install nodejs npm && \
    npm install -g grunt-cli && \
    cd app; npm install grunt; \
