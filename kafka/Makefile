
it:	clean network zookeeper kafka
volumes:	kafka-vol zookeeper-vols
all:	volumes it
x:	clean network volumes zookeeper

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
		--env ZOO_MY_ID=$$(hostname|cut -c 6-) \
		-v /home/lutz/zookeeper:/conf \
		-v zoo-data:/data \
		-v zoo-datalog:/datalog \
		zookeeper


kafka:
	docker run -d \
		--name kafka \
		--network kafka-net \
		--network-alias kafka \
		--env HOSTNAME_COMMAND="docker info 2>/dev/null | grep ^Name: | cut -d' ' -f 2" \
		--env KAFKA_BROKER_ID=10$$(hostname | cut -c 6-) \
		--env KAFKA_ZOOKEEPER_CONNECT=zookeeper:2181 \
		--env KAFKA_LISTENERS=INTERNAL://:9092,EXTERNAL://:9093 \
		--env advertised.listeners=INTERNAL://kafka:9092,EXTERNAL://$$(hostname):9093 \
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
	-docker network create kafka-net
	-docker network create --driver overlay zoonet


zoochen:
	-docker rm -f $$(docker ps -aq)
	-docker volume rm $$(docker volume ls -q | grep "\-data")
	-docker service rm $$(docker service ls -q)
	for i in 1 2 3 ; do  \
		docker volume create z$$i-data; docker volume create z$$i-datalog; \
		docker service create \
			--network zoonet \
			--name zoo$$i \
			--publish 218$$i:2181 \
			--env ZOO_MY_ID=$$i\
			--mount type=bind,source=/home/lutz/zookeeper/my-docker-entrypoint.sh,dst=/docker-entrypoint.sh \
			--mount type=bind,source=/home/lutz/zookeeper,dst=/conf \
			--mount type=volume,source=z$$i-data,dst=/data \
			--mount type=volume,source=z$$i-datalog,dst=/datalog \
			zookeeper ; \
	done

