FROM zenika/alpine-maven:latest

LABEL mfrunner-version=2.0.0

RUN mkdir /mfwf /in /out

RUN apk add --update curl bash \
&& rm -rf /var/cache/apk/*

RUN curl -L https://github.com/metafacture/metafacture-core/releases/download/metafacture-runner-2.0.0/metafacture-runner-2.0.0-dist.tar.gz \
2> /dev/null \
| tar -xz \
&& mv metafacture-runner-2.0.0 /app

ARG debuglevel=warn
RUN sed -i "13s/info/${debuglevel}/" /app/config/log4j.xml

VOLUME /app/plugins /mfwf /in /out
WORKDIR /mfwf

ENTRYPOINT ["/bin/bash", "/app/flux.sh"]
