# 📰 News Feed System

#tags: #systemdesign #hld #feed #important

## 📌 Requirements

### Functional:
- Show user feed
- Post updates

### Non-functional:
- High read throughput
- Low latency

---

## 📊 Estimation
- Heavy reads
- Millions of users

---

## 🔌 APIs
- POST /post
- GET /feed

---

## 🏗️ High-Level Design
- Feed generator service
- Cache layer
- DB
- Fan-out service

---

## 🗄️ Data Model
- user_id, post_id, timestamp

---

## 🔍 Deep Dive
- Fan-out on write vs read

---

## ⚖️ Tradeoffs
- Precompute vs compute on demand

---

## ⚠️ Bottlenecks
- Feed generation latency
