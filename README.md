# 📊 Hybrid Logging Setup with Grafana, Loki, Promtail, Elasticsearch, Filebeat & Logstash

This setup gives you the best of both worlds:

- 🔎 **Loki + Promtail**: Fast, lightweight log tailing for real-time dev/debugging.
- 🧠 **Filebeat + Logstash + Elasticsearch**: Full-text searchable, structured log ingestion for production-grade analysis.
- 📈 **Grafana**: Centralized log + metric visualization.

---

## 🧱 Architecture Overview

```
               ┌─────────────┐
               │ Application │
               └─────┬───────┘
                     │
       ┌─────────────┴─────────────┐
       │                           │
┌──────▼──────┐         ┌──────────▼────────┐
│   Alloy     │         │     Filebeat      │
└──────┬──────┘         └──────────┬────────┘
       │                           │
┌──────▼──────┐           ┌────────▼────────┐
│    Loki     │           │    Logstash     │
└──────┬──────┘           └────────┬────────┘
       │                           │
       └────────────┬──────────────┘
                    ▼
             ┌─────────────┐
             │Elasticsearch│
             └─────────────┘
                    │
                    ▼
               ┌────────┐
               │ Grafana│
               └────────┘
```

---

## 🐳 Docker Compose – Full Setup

Extend your `docker-compose.yml` with the following:


---

## 🔧 Configuration Files

### 📁 `./loki/loki-config.yaml`
loki config

---

### 📁 `./alloy/config.alloy`
use a protmail config and convert to alloy yaml - see https://grafana.com/docs/alloy/latest/set-up/migrate/from-promtail/

---

### 📁 `./logstash/logstash.conf`
logstash config

---

### 📁 `./filebeat/filebeat.yml`
filebeat config

---

## 🧪 How to Test the Logging Pipeline

### Pre setup actions
Use WSL2 on windows and change permissions of filebeat.yml with chmod 444 /mnt/c/.../filebeat.yml

1. **Start all containers**:
   ```bash
   docker-compose up -d
   ```

2. **Generate a test log**:
   ```bash
   echo "Test log entry $(date)" >> /var/log/test.log
   ```

3. **Access Grafana** at [http://localhost:3000](http://localhost:3000)  
   Login: `admin` / `admin`

4. **Add Data Sources**:
   - **Loki**: `http://loki:3100`
   - **Prometheus**: `http://prometheus:9090`
   - **Elasticsearch**: `http://elasticsearch:9200`
     - Index pattern: `logstash-*`
     - Time field: `@timestamp`

5. **Explore logs** in Grafana:
   - Loki: `{job="varlogs"}`
   - Elasticsearch: Full-text search via log fields

---

## ✅ Summary

| Component     | Purpose                                      |
|---------------|----------------------------------------------|
| **Promtail**  | Reads local logs → sends to **Loki**         |
| **Filebeat**  | Reads local logs → sends to **Logstash**     |
| **Logstash**  | Parses, filters → sends to **Elasticsearch** |
| **Grafana**   | Visualize everything 🎉                      |
