FROM debian:buster

ENV TESTE ""

RUN apt-get update && \
    apt-get install -y vim \
    curl && \
    apt-get autoremove -y && \
    apt-get auto-clean && \
    rm -rf /var/lib/apt/lists/*

COPY script.sh /etc/entrypoint.sh

RUN ["chmod", "+x", "/etc/entrypoint.sh"]

EXPOSE 80

ENTRYPOINT [ "/bin/bash", "/etc/entrypoint.sh" ]