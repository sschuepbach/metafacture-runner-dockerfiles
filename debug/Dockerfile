FROM zenika/alpine-maven:latest

LABEL mfrunner-version=5.0.0

RUN mkdir /mfwf /in /out

RUN apk add --update curl bash \
&& rm -rf /var/cache/apk/*

RUN curl -L https://github.com/metafacture/metafacture-core/releases/download/metafacture-core-5.0.0/metafacture-core-5.0.0-dist.tar.gz \
2> /dev/null \
| tar -xz \
&& mv metafacture-core-5.0.0-dist /app

ARG debuglevel=debug
RUN sed -i "13s/info/${debuglevel}/" /app/config/log4j.xml

ARG debugport=50505
ENV MFRUNNER=/app \
_JAVA_OPTIONS=-agentlib:jdwp=transport=dt_socket,server=y,suspend=y,address=${debugport}

EXPOSE $debugport
VOLUME /app/plugins /mfwf /in /out
WORKDIR /mfwf

ENTRYPOINT ["/bin/bash", "/app/flux.sh"]
