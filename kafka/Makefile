
it:	clean network zookeeper kafka
volumes:	kafka-vol zookeeper-vols
	docker volume ls

clean:
	-docker rm -f zookeeper
	-docker rm -f kafka
	-docker network rm kafka-net


zookeeper:
	docker run -d \
		--name zookeeper \
		--network kafka-net \
		--network-alias zookeeper \
		--publish 2181:2181 \
		--publish 2888:2888 \
		--publish 3888:3888 \
		-v /home/lutz/zookeeper:/conf \
		-v zoo-data:/data \
		-v zoo-datalog:/datalog \
		zookeeper


kafka:
	docker run -d \
		--name kafka \
		--network kafka-net \
		--network-alias kafka_container1 \
		--env KAFKA_ZOOKEEPER_CONNECT=zookeeper:2181 \
		--env KAFKA_LISTENERS=INTERNAL://:9092,EXTERNAL://:9093 \
		--publish 9093:9093 \
		-v /home/lutz/kafka/logs:/tmp/kafka-logs \
		-v /home/lutz/kafka/config:/opt/kafka/config \
		-v kafka-vol:/kafka \
		wurstmeister/kafka

kafka-vol:
	-docker volume rm kafka-vol
	docker volume create kafka-vol

zookeeper-vols:
	-docker volume rm zoo-data
	-docker volume rm zoo-datalog
	docker volume create zoo-data
	docker volume create zoo-datalog

network:
	docker network create kafka-net

