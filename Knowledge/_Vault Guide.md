# 🧠 Vault Guide (AI + Human Readable)

#tags: #meta #vault #guide

---

## 🎯 Purpose

This document explains the **structure of the vault** so that:
- Humans can navigate efficiently
- AI agents can understand relationships between topics

---

## 📁 Folder Structure

### 📂 Knowledge/

#### 🔹 DSA/
- Core problem solving notes
- Pattern-based learning
- Competitive coding concepts

Subfolders:
- Notes/ → Atomic topic notes (LIS, Subset Sum, etc.)
- Revision/ → Dashboard + revision system

---

#### 🔹 System Design/

Subfolders:
- Components/ → Reusable building blocks
  - Caching
  - Load Balancer
  - CDN
  - Message Queue
  
- Systems/ → Full design problems
  - URL Shortener
  - Chat System
  - News Feed
  
- Templates/ → Reusable templates
  - HLD Template

- index.md → Entry point for HLD

---

## 🧩 Note Philosophy

Each note follows:

- One concept per file
- Clear headings
- Tags for classification
- Links to related notes

---

## 🏷️ Tag System

### 📌 Core Tags
- #dsa
- #systemdesign
- #hld

### 📌 Concept Tags
- #dp
- #graph
- #heap
- #cache
- #queue

### 📌 Utility Tags
- #important
- #revision
- #template

---

## 🔗 Linking Rules

- Use [[Wiki Links]] between related concepts
- Example:
  - [[Caching]] ↔ [[LRU Cache]]
  - [[Message Queue]] ↔ [[Chat System]]

---

## 🔁 Revision System

- Located in: DSA/Revision/
- Uses:
  - Priority buckets (High / Medium / Low)
  - Spaced repetition

---

## 🧠 AI Agent Instructions

When reading this vault:

1. Start at:
   - System Design → index.md
   - DSA → Revision Dashboard

2. Traverse via links:
   - Components → Systems
   - Patterns → Problems

3. Prioritize:
   - #important notes
   - High-priority revision topics

4. Maintain context:
   - DSA ↔ LLD ↔ HLD connections

---

## 🚀 Future Extensions

- Add LLD folder
- Add interview experiences
- Add mock tracking

---

## 🧠 Key Principle

This vault is designed as a:

> Connected Knowledge Graph (not linear notes)

