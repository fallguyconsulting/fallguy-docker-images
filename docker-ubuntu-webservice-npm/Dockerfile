FROM ubuntu:bionic

WORKDIR /app

RUN apt-get update          \
    && apt-get -y --no-install-recommends install \
        ca-certificates     \
        curl                \
        gcc                 \
        g++                 \
        git                 \
        make                \
        openssl             \
        nano                \
        vim

RUN git clone -b OpenSSL_1_1_1b https://github.com/openssl/openssl.git \
    && cd openssl           \
    && ./config             \
    && make                 \
    && make install

RUN curl -sL https://deb.nodesource.com/setup_14.x | bash - \
    && apt-get install -y nodejs

EXPOSE 7777
ENTRYPOINT [ "npm" ]
CMD [ "run", "prod" ]
