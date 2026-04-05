# 🎥 Video Streaming System

#tags: #systemdesign #hld #cdn #important

## 📌 Requirements

### Functional:
- Upload video
- Stream video

### Non-functional:
- High bandwidth
- Low latency playback
- High availability

---

## 📊 Estimation
- Very high storage
- High read traffic

---

## 🔌 APIs
- POST /upload
- GET /stream

---

## 🏗️ High-Level Design
- Upload service
- Transcoding service
- CDN
- Storage (S3)

---

## 🗄️ Data Model
- video_id, metadata, URL

---

## 🔍 Deep Dive
- Chunking videos
- Adaptive bitrate streaming

---

## ⚖️ Tradeoffs
- Cost vs performance

---

## ⚠️ Bottlenecks
- Bandwidth usage
