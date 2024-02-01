ARG PG_MAJOR=16
FROM postgres:$PG_MAJOR
ARG PG_MAJOR

# China users can use the following commands to speed up the build process
# RUN sed -i 's/apt.postgresql.org\/pub/mirrors.tuna.tsinghua.edu.cn\/postgresql/g' /etc/apt/sources.list.d/pgdg.list
# RUN sed -i 's/deb.debian.org/mirrors.aliyun.com/g' /etc/apt/sources.list
# RUN sed -i 's/security.debian.org/mirrors.aliyun.com/g' /etc/apt/sources.list

SHELL ["/bin/bash", "-c"]
RUN set -o pipefail && \
    apt-get update && \
    apt-mark hold locales && \
    apt-get -y install --no-install-recommends apt-transport-https build-essential postgresql-server-dev-$PG_MAJOR wget git && \
    cd /tmp && \
    wget http://www.xunsearch.com/scws/down/scws-1.2.3.tar.bz2 -O scws-1.2.3.tar.bz2 && \
    tar xvjf scws-1.2.3.tar.bz2 && \
    cd scws-1.2.3 && ./configure --prefix=/usr/local/scws && make install && cd .. && \
    wget http://www.xunsearch.com/scws/down/scws-dict-chs-gbk.tar.bz2 && \
    tar xvjf scws-dict-chs-gbk.tar.bz2 && \
    wget http://www.xunsearch.com/scws/down/scws-dict-chs-utf8.tar.bz2 && \
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
