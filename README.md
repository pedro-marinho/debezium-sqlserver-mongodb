# Start Kafka and Databases
docker-compose up

# Initialize database and insert test data
rodar script.sql

# Start SQL Server connector
curl -i -X POST -H "Accept:application/json" -H  "Content-Type:application/json" http://localhost:8083/connectors/ -d @register-sqlserver.json

# Consume messages from a Debezium topic
docker-compose exec kafka /kafka/bin/kafka-console-consumer.sh --bootstrap-server kafka:9092 --from-beginning --property print.key=true --topic server1.dbo.customers

# Start MongoDB Sink Connector
confluent-hub install mongodb/kafka-connect-mongodb:1.3.0

curl -i -X POST -H "Accept:application/json" -H  "Content-Type:application/json" http://localhost:8084/connectors/ -d @register-mongo-sink.json

# Shut down the cluster
docker-compose down