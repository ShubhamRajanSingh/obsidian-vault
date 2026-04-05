# 🔗 URL Shortener

#tags: #systemdesign #hld #important

## 📌 Requirements

### Functional:
- Shorten long URL
- Redirect to original URL

### Non-functional:
- High read traffic
- Low latency
- High availability

---

## 📊 Estimation
- Reads >> Writes
- Millions of requests/day

---

## 🔌 APIs
- POST /shorten
- GET /{shortUrl}

---

## 🏗️ High-Level Design
- API Layer
- Application Service
- Key-Value DB
- Cache (Redis)

---

## 🗄️ Data Model
- short_url → original_url

---

## 🔍 Deep Dive
- ID generation (Base62)
- Cache hot URLs

---

## ⚖️ Tradeoffs
- Consistency vs performance

---

## ⚠️ Bottlenecks
- DB scaling
- Hot keys
