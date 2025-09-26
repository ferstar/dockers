ARG PG_MAJOR=18
FROM postgres:18beta1-bookworm
ARG PG_MAJOR

SHELL ["/bin/bash", "-c"]

RUN set -o pipefail && \
    sed -i 's#apt.postgresql.org/pub#mirrors.tuna.tsinghua.edu.cn/postgresql#g' /etc/apt/sources.list.d/pgdg.list && \
    sed -i 's#deb.debian.org/debian#mirrors.tuna.tsinghua.edu.cn/debian#g' /etc/apt/sources.list.d/debian.sources && \
    sed -i 's#deb.debian.org/debian-security#mirrors.aliyun.com/debian-security#g' /etc/apt/sources.list.d/debian.sources && \
    apt-get update && \
    apt-mark hold locales && \
    apt-get -y install --no-install-recommends apt-transport-https build-essential postgresql-server-dev-$PG_MAJOR wget git && \
    cd /tmp && \
    wget --no-check-certificate https://www.xunsearch.com/scws/down/scws-1.2.3.tar.bz2 -O scws-1.2.3.tar.bz2 && \
    tar xvjf scws-1.2.3.tar.bz2 && \
    cd scws-1.2.3 && ./configure --prefix=/usr/local/scws && make install && cd .. && \
    wget --no-check-certificate https://www.xunsearch.com/scws/down/scws-dict-chs-gbk.tar.bz2 && \
    tar xvjf scws-dict-chs-gbk.tar.bz2 && \
    wget --no-check-certificate https://www.xunsearch.com/scws/down/scws-dict-chs-utf8.tar.bz2 && \
    tar xvjf scws-dict-chs-utf8.tar.bz2 && \
    mv *.xdb /usr/local/scws/etc/ && \
    git config --global http.sslverify false && \
    git clone https://github.com/amutu/zhparser.git --depth=1 && \
    cd zhparser && SCWS_HOME=/usr/local/scws make && make install && cd .. && \
    git clone https://github.com/pgvector/pgvector.git --depth=1 && \
    cd pgvector && \
    make clean && \
    make OPTFLAGS="" && \
    make install && \
    apt-get remove -y --purge apt-transport-https build-essential wget git postgresql-server-dev-$PG_MAJOR && \
    apt-get autoremove -y && \
    apt-mark unhold locales && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
