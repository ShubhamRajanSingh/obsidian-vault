# 💬 Chat System

#tags: #systemdesign #hld #realtime #important

## 📌 Requirements

### Functional:
- Send/receive messages
- One-to-one + group chat

### Non-functional:
- Low latency
- Real-time delivery
- High availability

---

## 📊 Estimation
- High write + read
- Concurrent users

---

## 🔌 APIs
- POST /sendMessage
- GET /messages

---

## 🏗️ High-Level Design
- WebSocket servers
- Message service
- Queue (Kafka)
- DB (NoSQL)

---

## 🗄️ Data Model
- user_id, message_id, content, timestamp

---

## 🔍 Deep Dive
- WebSocket vs polling
- Message ordering

---

## ⚖️ Tradeoffs
- Consistency vs latency

---

## ⚠️ Bottlenecks
- Message delivery at scale
