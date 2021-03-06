FROM gcr.io/google-appengine/debian9
ENV PROMETHEUS_VERSION 2.17.2

ENV PROMETHEUS_URL="https://github.com/prometheus/prometheus/releases/download/v$PROMETHEUS_VERSION/prometheus-$PROMETHEUS_VERSION.linux-amd64.tar.gz"
ENV PROMETHEUS_SHA256 a8ca17b845df4967ff5ef9f9b2c7c98568104e777c9f9a4eb1bc409726089626

ENV C2D_RELEASE ${PROMETHEUS_VERSION}

RUN set -x && \
    apt-get update && apt-get install -qq -y wget && \
    mkdir -p /etc/prometheus && \
    mkdir prometheus-files && cd prometheus-files && \
    wget -O prometheus.tar.gz "$PROMETHEUS_URL" && \
    echo "$PROMETHEUS_SHA256 prometheus.tar.gz" | sha256sum -c - && \
    tar xfvz prometheus.tar.gz --strip-components=1 && \
    mv prometheus          /bin/ && \
    mv promtool            /bin/ && \
    mv prometheus.yml      /etc/prometheus/ && \
    mv console_libraries   /usr/share/prometheus/ && \
    mv consoles            /usr/share/prometheus/ && \
    cd - && \
    rm -r prometheus-files && \
    ln -s /usr/share/prometheus/console_libraries /usr/share/prometheus/consoles/ /etc/prometheus && \
    mkdir -p /prometheus && \
    chown -R nobody:nogroup /etc/prometheus /prometheus

USER       nobody
EXPOSE     9090
VOLUME     [ "/prometheus" ]
WORKDIR    /prometheus
ENTRYPOINT [ "/bin/prometheus" ]
CMD        [ "--config.file=/etc/prometheus/prometheus.yml", \
             "--storage.tsdb.path=/prometheus", \
             "--web.console.libraries=/usr/share/prometheus/console_libraries", \
             "--web.console.templates=/usr/share/prometheus/consoles" ]

