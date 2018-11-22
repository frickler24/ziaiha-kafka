
it:	clean volumes k2
volumes:	kafka-vol

clean:
	-docker rm -f kafka


kafka-vol:
	-docker volume rm kafka-vol
	docker volume create kafka-vol

webui:
	docker run -d \
		-p 80:8080 \
		-e ZK_DEFAULT_NODE=localhost:2181/ \
		--name zk-web \
		-t tobilg/zookeeper-webui

k1:
	docker run -it --rm \
		--name kafka \
		-p 9092:9092 \
		-p 9093:9093 \
		--env KAFKA_BROKER_ID=9001 \
		--env KAFKA_ZOOKEEPER_CONNECT=emanuel:2181 \
		--env KAFKA_LISTENERS="INTERNAL://:9092,EXTERNAL://emanuel:9093" \
		--env advertised.listeners="INTERNAL://:9092,EXTERNAL://emanuel:9093" \
		-v /home/pi/ziaiha-kafka/kafka/logs:/tmp/kafka-logs \
		-v /home/pi/ziaiha-kafka/kafka/config:/usr/local/kafka/config \
		-v k1-kafka-vol:/kafka \
		kafka bash
	@echo "Starte Kafka Broker mit "kafka-server-start.sh config/server.properties"

k2:
	docker run -it --rm \
		--name kafka \
		-p 9092:9092 \
		-p 9093:9093 \
		--env KAFKA_BROKER_ID=9002 \
		--env KAFKA_ZOOKEEPER_CONNECT=zoo1:2181 \
		--env KAFKA_LISTENERS="INTERNAL://:9092,EXTERNAL://lutz:9093" \
		--env advertised.listeners="INTERNAL://:9092,EXTERNAL://lutz:9093" \
		-v /home/pi/ziaiha-kafka/kafka/logs:/tmp/kafka-logs \
		-v /home/pi/ziaiha-kafka/kafka/config:/usr/local/kafka/config \
		-v k1-kafka-vol:/kafka \
		kafka bash
	@echo "Starte Kafka Broker mit "kafka-server-start.sh config/server.properties"


