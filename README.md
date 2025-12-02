# Real-Time Trading Platform – Technical Portfolio

This repository contains a short technical portfolio showcasing the real-time
market data infrastructure and multi-chart trading platform I developed 
end-to-end over the last years using C++, Python and multithreaded architectures.

---

## 📄 Portfolio PDF

👉 [Download the Technical Portfolio](./Trading_Platform_Portfolio.pdf)

---

## 🚀 Overview

The system is composed of four main components:

1. **FeedDaemon**  
   Connects to real-time market data sources (TradeStation, IQFeed or SQLite DB),
   decodes provider-specific messages, and produces normalized bar/tick events.

2. **MetaServer (Data Hub)**  
   A high-performance, multithreaded server responsible for event processing, 
   caching, throttling and distribution to connected clients.

3. **MetaClient (Distribution Layer)**  
   A lightweight agent that receives events from MetaServer and forwards them 
   to all active instances of the trading platform, ensuring synchronization 
   and real-time consistency.

4. **Quantum Gann Station (Trading GUI)**  
   A standalone multi-chart platform receiving live updates, rendering complex 
   indicators, and providing an operational interface for analysis and trading.

---

## ⚙️ MetaServer Architecture

The MetaServer implements a multithreaded, high-throughput data pipeline:

- **Feed Thread**  
  - Connects to the data feed  
  - Parses raw provider messages  
  - Normalizes events (bar updates, open/close)  
  - Writes updates into a lock-protected cache (semaphore)

- **Main Dispatcher Thread**  
  - Reads the cache in shared non-blocking mode  
  - Pushes events to all connected clients  
  - Removes delivered events from the queue

- **Client Threads**  
  - One per connected client  
  - Sleep when idle, wake on new events  
  - Handle ping/heartbeat  
  - Monitor latency and synchronization status

- **Dynamic Throttling**  
  If client queues grow too fast (market bursts, high volatility), 
  the system automatically drops non-critical events while always preserving  
  essential ones (bar open/close).

---

## 🔌 MetaClient Responsibilities

The MetaClient acts as a distributed gateway between the data server 
and the trading GUI:

- Receives real-time updates (bar update, open/close, subscriptions)  
- Forwards events to **multiple active platform instances**  
- Performs lightweight buffering and ordering  
- Sends periodic ping/heartbeat to MetaServer  
- Executes fast catch-up logic when lagging  
- Monitors the number of active QGS windows

---

## 📊 Quantum Gann Station (QGS)

A multi-chart C++/FLTK trading platform capable of:

- Rendering multiple instruments and timeframes simultaneously  
- Displaying advanced indicators (Gann tools, overlays, mathematical studies)  
- Reacting instantly to incoming data events  
- Handling strategy visualization and analysis  
- Operating fully real-time when connected to MetaClient

---

## 🧩 Data Flow Summary

Data Feed (IQFeed / TradeStation / SQLite)
↓
FeedDaemon
↓
MetaServer
(cache + dispatch)
↓
MetaClient
↓
Quantum Gann Station (multiple instances)


---

## 👤 About the Author

I am a senior C++ and trading systems developer with over 30 years of experience, 
10+ of which focused on algorithmic trading, real-time systems, data pipelines, 
platform architecture and multi-machine infrastructure.

Looking for senior C++/trading systems roles, remote or contract.




