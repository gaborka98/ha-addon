ARG BUILD_FROM
FROM ${BUILD_FROM}

ENV LANG=C.UTF-8
ENV CK_API_ENV=prod


RUN apk add --no-cache \
    nodejs \
    npm \
    python3 \
    build-base

RUN apk add --no-cache wget tar && \
    mkdir -p /root/.node-gyp && \
    wget -q https://nodejs.org/download/release/v24.11.1/node-v24.11.1-headers.tar.gz && \
    tar -xzf node-v24.11.1-headers.tar.gz -C /root/.node-gyp --strip-components=1 && \
    rm node-v24.11.1-headers.tar.gz
ENV npm_config_nodedir=/root/.node-gyp

WORKDIR /workspace
RUN mkdir -p /workspace/node_modules

COPY ["./dist", "./package.json", "/workspace/"]
RUN npm install --production --save-exact
COPY ["./node_modules", "/workspace/node_modules/"]

COPY docker-entrypoint.sh /usr/local/bin/
RUN chmod +x /usr/local/bin/docker-entrypoint.sh

ENTRYPOINT ["/usr/local/bin/docker-entrypoint.sh"]
EXPOSE 3000
CMD ["npm", "start"]