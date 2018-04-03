FROM danisla/hadoop:2.7.3

ENV VERSION 1.3.1
ENV DESTINATION /opt/hbase

WORKDIR /

ADD http://archive.apache.org/dist/hbase/${VERSION}/hbase-${VERSION}-bin.tar.gz /
RUN tar -xvf /hbase-${VERSION}-bin.tar.gz
RUN ls /
RUN mv /hbase-${VERSION} ${DESTINATION}

# REST API
EXPOSE 8080
# Thrift API
EXPOSE 9090

# Zookeeper port
EXPOSE 2181

# Master port
EXPOSE 16000
# Master info port
EXPOSE 16010

# Regionserver port
EXPOSE 16020
# Regionserver info port
EXPOSE 16030