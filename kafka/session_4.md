
# Apache Kafka Hand's On

## 📥 Download and Extract Kafka

```bash
1. Download Kafka from https://kafka.apache.org/downloads
2. tar -xzf kafka_2.13-4.0.0.tgz
3. cd kafka_2.13-4.0.0
4. Reference: https://kafka.apache.org/quickstart
```

---

## 🚀 Start Kafka Cluster

```bash
5. KAFKA_CLUSTER_ID="$(bin/kafka-storage.sh random-uuid)"
6. bin/kafka-storage.sh format --standalone -t $KAFKA_CLUSTER_ID -c config/server.properties
7. bin/kafka-server-start.sh config/server.properties
```

---

## 🧵 Topic Creation

```bash
8. bin/kafka-topics.sh --create --topic go_session --bootstrap-server localhost:9092
9. bin/kafka-topics.sh --describe --topic go_session --bootstrap-server localhost:9092
```

---

## ✍️ Start a Producer

```bash
10. bin/kafka-console-producer.sh --topic go_session --bootstrap-server localhost:9092
```

---

## 📥 Start a Consumer

```bash
11. bin/kafka-console-consumer.sh --topic go_session --from-beginning --bootstrap-server localhost:9092
```

---

## 📤 Publish Data Using Producer

```bash
12. echo '{"userId": "1", "userType": "Instuctor", "message" : "Session Started", "timestamp": "2025-07-19T14:30:00Z"}' \
| bin/kafka-console-producer.sh --topic go_session --bootstrap-server localhost:9092
```

---

## 🧩 Create Topic with 2 Partitions

```bash
13. bin/kafka-topics.sh --create --topic go_session_2 --partitions 2 --replication-factor 1 --bootstrap-server localhost:9092
```

---

## 👥 Start Consumers in a Consumer Group

```bash
14. bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic go_session_2 --group go_team_shiksha --from-beginning
15. bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic go_session_2 --group go_team_shiksha --from-beginning
```
```text
What if we don't provide --from-beginning
```
---

## 📨 Publish Keyed Messages to Partitioned Topic

**Kafka Partition Logic:**

```text
partition = hash(key) % number_of_partitions
```

```bash
16. echo "1:{\"userId\": \"1\", \"userType\": \"Instuctor\", \"message\" : \"Session Started\", \"timestamp\": \"2025-07-19T14:30:00Z\"}" \
| bin/kafka-console-producer.sh --topic go_session_2 --bootstrap-server localhost:9092 \
--property "parse.key=true" --property "key.separator=:"

17. echo "2:{\"userId\": \"1\", \"userType\": \"Instuctor\", \"message\" : \"Session Started\", \"timestamp\": \"2025-07-19T14:30:00Z\"}" \
| bin/kafka-console-producer.sh --topic go_session_2 --bootstrap-server localhost:9092 \
--property "parse.key=true" --property "key.separator=:"

18. echo "5:{\"userId\": \"1\", \"userType\": \"Instuctor\", \"message\" : \"Session Started\", \"timestamp\": \"2025-07-19T14:30:00Z\"}" \
| bin/kafka-console-producer.sh --topic go_session_2 --bootstrap-server localhost:9092 \
--property "parse.key=true" --property "key.separator=:"
```

---

## 📊 Check Partition Assignment to Consumers

```bash
# Syntax
bin/kafka-consumer-groups.sh --bootstrap-server localhost:9092 --describe --group <group_name>

# Example
bin/kafka-consumer-groups.sh --bootstrap-server localhost:9092 --describe --group go_team_shiksha
```

---

### ✅ Notes:
- Kafka assigns **one partition to one consumer** per group.
- If partitions > consumers, some consumers get more partitions.
- If consumers > partitions, extra consumers stay idle.