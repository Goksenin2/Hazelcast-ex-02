# Docker Demos: Kafka & Hazelcast

A compact set of examples to run Apache Kafka (with ZooKeeper) and Hazelcast (with Management Center) locally using Docker.

## Prerequisites

- Docker & Docker Compose installed  
- At least 2 GB RAM allocated to Docker  
- A terminal and a web browser  

## Kafka Demo

1. Start ZooKeeper & Kafka  
   ```bash
   docker compose -f kafka-compose.yml up -d

 2. Create topic
    docker exec kafka \
      kafka-topics.sh --create \
        --topic test-topic \
        --bootstrap-server localhost:9092 \
        --partitions 1 \
        --replication-factor 1

  3. Produce a message
     docker exec -it kafka \
       kafka-console-producer.sh \
         --topic test-topic \
         --bootstrap-server localhost:9092

  4. Consume the message
     docker exec kafka \
       kafka-console-consumer.sh \
         --topic test-topic \
         --from-beginning \
         --bootstrap-server localhost:9092 \
         --max-messages 1

 ## Hazelcast Demo

 1. Create a Docker network
    docker network create hazelcast-net

 2. Run Hazelcast server
    docker run -d --name hazelcast-server \
       --network hazelcast-net \
       -e HZ_CLUSTER_NAME=dev \
       hazelcast/hazelcast:latest

  3. Launch Management Center
     docker run -d --rm --name hazelcast-mancenter \
       --network hazelcast-net \
       -p 8080:8080 \
       hazelcast/management-center:latest

   4. In your browser go to http://localhost:8080

         Click DEV MODE → ENABLE

         Go to Clusters → + Add Cluster

         Cluster Name: dev

         Member Address: hazelcast-server:5701
