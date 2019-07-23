FROM openjdk:8

RUN apt-get update
RUN apt-get -y install supervisor python-pip
RUN pip install supervisor-stdout
RUN mkdir -p /var/log/supervisor
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf

ENV VERSION 2.0.0
ENV DESTINATION /opt/hbase

ADD http://archive.apache.org/dist/hbase/${VERSION}/hbase-${VERSION}-bin.tar.gz /hbase-${VERSION}-bin.tar.gz
RUN tar -xvf hbase-${VERSION}-bin.tar.gz 
RUN mv /hbase-${VERSION} ${DESTINATION}
ADD hbase-site.xml /${DESTINATION}/conf/hbase-site.xml

COPY phoenix-5.0.0-HBase-2.0-server.jar ${DESTINATION}/lib/phoenix-5.0.0-HBase-2.0-server.jar
COPY phoenix-5.0.0-HBase-2.0-queryserver.jar ${DESTINATION}/lib/phoenix-5.0.0-HBase-2.0-queryserver.jar

# Folder for data
RUN mkdir -p /data/hbase
RUN mkdir -p /data/zookeeper

ENV JAVA_HOME /usr/local/openjdk-8
ENV PATH $PATH:/${DESTINATION}/bin

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

VOLUME /data/hbase
VOLUME /data/zookeeper
WORKDIR ${DESTINATION}


CMD ["/usr/bin/supervisord"]



