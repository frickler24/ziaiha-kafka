FROM arm32v6/alpine

LABEL maintainer="frickler24@frickler24.de"

RUN apk update \
	&& apk upgrade \
	&& apk add --no-cache \
		openjdk8 \
		bash

# Dann als nächstes kafka laden und installieren
# Da wir den gesamten Verschlüsselungskram nicht geladen haben, holen wir über unsicheres http
# NICHT FÜR PROD NUTZEN!!
# RUN wget http://www-eu.apache.org/dist/kafka/2.0.1/kafka_2.11-2.0.1.tgz
COPY kafka_2.11-2.0.1.tgz .

RUN tar -zxvf kafka_2.11-2.0.1.tgz -C /usr/local/ \
	&& mv /usr/local/kafka_2.11-2.0.1/ /usr/local/kafka \
	&& rm -f kafka_2.11-2.0.1.tgz

ENV PATH=/usr/local/kafka/bin:$PATH

WORKDIR /usr/local/kafka

COPY _usr_local_kafka_bin_kafka-run-class.sh bin/kafka-run-class.sh
COPY _usr_local_kafka_bin_kafka-server-start.sh bin/kafka-server-start.sh

EXPOSE 9092

# Starte Kafka über das üblich Startscript. Environment ist von außen zu setzen
CMD /usr/local/kafka/bin/kafka-server-start.sh  /usr/local/kafka/config/server.properties

