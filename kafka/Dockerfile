# Kafka and Zookeeper

FROM java:openjdk-8-jre

ENV DEBIAN_FRONTEND noninteractive

# supervisor, dnsutils
RUN apt-get update && \
    apt-get install -y wget supervisor dnsutils && \
    rm -rf /var/lib/apt/lists/* 
     
# zookeeper
ENV ZOOKEEPER_VERSION 3.4.6
ENV ZOOKEEPER_RELEASE zookeeper-$ZOOKEEPER_VERSION
RUN echo $ZOOKEEPER_RELEASE
RUN wget -q -P /tmp http://www.gtlib.gatech.edu/pub/apache/zookeeper/$ZOOKEEPER_RELEASE/$ZOOKEEPER_RELEASE.tar.gz
RUN tar xfz /tmp/$ZOOKEEPER_RELEASE.tar.gz -C /opt && rm /tmp/$ZOOKEEPER_RELEASE.tar.gz
ENV ZOOKEEPER_HOME /opt/$ZOOKEEPER_RELEASE
RUN cp $ZOOKEEPER_HOME/conf/zoo_sample.cfg $ZOOKEEPER_HOME/conf/zoo.cfg
ENV PATH $ZOOKEEPER_HOME/bin:$PATH

# kafka
ENV KAFKA_VERSION 0.8.2.1
ENV SCALA_VERSION 2.10
ENV KAFKA_RELEASE kafka_$SCALA_VERSION-$KAFKA_VERSION
RUN wget -q -P /tmp http://mirrors.koehn.com/apache/kafka/$KAFKA_VERSION/$KAFKA_RELEASE.tgz
RUN tar xfz /tmp/$KAFKA_RELEASE.tgz -C /opt && rm /tmp/$KAFKA_RELEASE.tgz
ENV KAFKA_HOME /opt/$KAFKA_RELEASE
# NOTE: You need to run this image with -h kafka for this to work
ENV ADVERTISED_HOST kafka

ADD scripts/start-kafka.sh /usr/bin/start-kafka.sh

# Supervisor config
ADD supervisor/kafka.conf /etc/supervisor/conf.d/kafka.conf
ADD supervisor/zookeeper.conf /etc/supervisor/conf.d/zookeeper.conf

# 2181 is zookeeper, 9092 is kafka
EXPOSE 2181 9092

CMD ["supervisord", "-n"]
