# 🚗 Ride Sharing System (Uber)

#tags: #systemdesign #hld #realtime #important

## 📌 Requirements

### Functional:
- Book ride
- Match drivers
- Track location

### Non-functional:
- Real-time updates
- Low latency
- High availability

---

## 📊 Estimation
- High concurrency
- Frequent location updates

---

## 🔌 APIs
- POST /requestRide
- GET /trackRide

---

## 🏗️ High-Level Design
- Location service
- Matching service
- Notification service
- Map service

---

## 🗄️ Data Model
- user_id, driver_id, location, ride_id

---

## 🔍 Deep Dive
- Driver matching algorithms
- Location indexing (GeoHash)

---

## ⚖️ Tradeoffs
- Accuracy vs latency

---

## ⚠️ Bottlenecks
- Matching at scale
