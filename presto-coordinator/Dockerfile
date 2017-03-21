FROM openjdk:8-jre

RUN apt-get update -y \
 && apt-get install -y python-pip \
 && pip install envtpl \
 && apt-get install -y uuid

ENV PRESTO_VERSION 0.166
ENV PRESTO_DIR /opt/presto
ENV PRESTO_ETC_DIR /etc/presto
ENV PRESTO_DATA_DIR /home/presto-data

RUN mkdir -p ${PRESTO_DIR} ${PRESTO_ETC_DIR}/catalog \
 && curl -s http://central.maven.org/maven2/com/facebook/presto/presto-server/${PRESTO_VERSION}/presto-server-${PRESTO_VERSION}.tar.gz \
 | tar --strip 1 -vxzC ${PRESTO_DIR} \
 && curl -s http://central.maven.org/maven2/com/facebook/presto/presto-cli/${PRESTO_VERSION}/presto-cli-${PRESTO_VERSION}-executable.jar > /usr/local/bin/presto \
 && chmod +x /usr/local/bin/presto

WORKDIR ${PRESTO_DIR}

RUN mkdir ${PRESTO_DATA_DIR}
ENV NODE_node.data-dir ${PRESTO_DATA_DIR}

RUN uid="$(uuid)"
ENV NODE_node.id $uid

RUN mkdir ${PRESTO_DIR}/etc
COPY ./etc ${PRESTO_DIR}/etc/

EXPOSE 8080

CMD ["/opt/presto/bin/launcher", "run"]
