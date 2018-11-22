
it:	clean zookeeper-vols zookeeper

clean:
	-docker rm -f zookeeper

zookeeper_gehtnichtmitPortfreigabe:
	@echo;echo "Starte zookeeper lokal mit ID $$ID"
	docker run -it --rm \
		--name zookeeper \
		--publish 2181:2181 \
		--publish 2888:2888 \
		--publish 3888:3888 \
		--env ZOO_MY_ID=$$ID \
		-v /home/pi/ziaiha-kafka/zookeeper1:/conf \
		-v zoo-data:/data \
		-v zoo-datalog:/datalog \
		zookeeper

zookeeper:
	@echo;echo "Starte zookeeper lokal mit ID $$ID"
	docker run -d \
		--name zookeeper \
		--network host \
		--env ZOO_MY_ID=$$ID \
		-v /home/pi/ziaiha-kafka/zookeeper1:/conf \
		-v zoo-data:/data \
		-v zoo-datalog:/datalog \
		zookeeper

zookeeper-vols:
	-docker volume rm zoo-data
	-docker volume rm zoo-datalog
	docker volume create zoo-data
	docker volume create zoo-datalog

