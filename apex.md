# 🚀 Technical Excellence Roadmap

A comprehensive roadmap organized by action type for achieving technical excellence in Software Engineering, Machine Learning, and Distributed Systems.

## Table of Contents
- [📝 Todo List](#-todo-list)
- [Core CS Concepts](#core-cs-concepts)
- [📚 Courses](#-courses)
- [📖 Books](#-books)
- [🛠️ Projects to Build](#️-projects-to-build)

## 📝 Todo List
- Solve 500+ LeetCode Hard problems (focus on graph theory, DP, bitmasking)
- Solve 50 problems from Google Code Jam Finals archives
- Optimize inference latency by 40% using quantization
- Fine-tune GPT-4 on niche datasets
- Debug microservice handling 10M RPS
- Profile Linux kernel bottlenecks using eBPF
- Contribute 200+ PRs to high-impact projects
- Organize workshops at major tech events

## 📚 Courses
- MIT 18.065: Matrix Methods in Data Analysis
- Stanford CS229: Machine Learning
- CS50 (Harvard) – Deep CS Fundamentals
- System Design Masterclass – Alex Xu
- LLMOps & AI Deployment – DeepLearning.AI
- MIT Advanced Algorithms (https://6.5210.csail.mit.edu/materials) (https://courses.csail.mit.edu/6.854/20/)
- MIT 6.172: Performance Engineering
- MIT 6.828: OS Engineering
- MIT 18.065 (Matrix Methods in Data Analysis)
- [System Design Primer](https://github.com/donnemartin/system-design-primer)


## 📖 Books
### Mathematics & Algorithms
- The Algorithm Design Manual – Steven Skiena
- Deep Learning – Ian Goodfellow, Yoshua Bengio, Aaron Courville
- Competitive Programming 4 – Steven Halim
- Grokking Algorithms – Aditya Bhargava
- Introduction to Algorithms – Cormen, Leiserson, Rivest, Stein
- Competitive Programmer’s Handbook

### Systems & Architecture
- Designing Data-Intensive Applications – Martin Kleppmann
- Database Internals – Alex Petrov
- The Art of Scalability – Martin Abbott
- Scalability Rules – Martin Abbott
- AI-Powered Search – Trey Grainger
- Computer Systems: A Programmer's Perspective – Bryant & O'Hallaron
- Neural Networks & Deep Learning – Michael Nielsen
- Efficient Processing of Deep Neural Networks – O'Reilly
- High-Performance MySQL – O'Reilly
- The Pragmatic Programmer – Andrew Hunt & David Thomas
- Distributed Systems: Principles and Paradigms – Tanenbaum
- Building Microservices – Sam Newman
- Linux Kernel Development – Robert Love
- Systems Performance: Enterprise and the Cloud – Brendan Gregg
- Designing for Scalability with Erlang/OTP - Francesco
- High-Performance Python – O'Reilly
- The Phoenix Project – Gene Kim
- Web Scalability for Startup Engineers – Artur Ejsmont
- Designing for Performance – Brendan Gregg
- Real-Time Data Streaming by Confluent
- Kubernetes in Production – Kelsey Hightower
- Three Easy Pieces – Remzi Arpaci-Dusseau

## 🛠️ Projects to Build

### ML/AI Projects
- Implement PCA, SVD, and Fourier Transforms from scratch (Python/C++)
- Build backpropagation for BERT/GPT without frameworks
- Train a multimodal LLM on custom dataset. Optimize inference latency to <50ms using TensorRT, quantization, and model distillation.
- Build personal AI assistant
- Rebuild Netflix's recommendation system with 20% higher accuracy
- Build a GPT-4 fine-tuned chatbot
- Fine-tune a Sentence Transformer for embeddings
- Smart Contracts & LLMs – Build AI-powered blockchain analytics
- Edge AI & WASM Optimization – Run AI models in the browser
- Optimize LLM inference with Flash Attention, Quantization (INT8), and Triton Inference Server.

### Systems Projects
- Build a distributed SQL database (+engine) from scratch (Golang/Rust)
- Implement Raft consensus in Go
- Implement distributed ACID transactions (2PC, 3PC, Sagas).
- Build globally distributed app using Kubernetes + Istio
- Write custom memory allocator (mimic jemalloc tcmalloc)
- Build multi-threaded HTTP server in Rust with zero-copy I/O
- Design WhatsApp-scale chat system (10M+ users)
- Create YouTube/Tiktok-scale CDN with edge computing + caching
- Benchmark WebSockets vs. gRPC vs. SSE for real-time feeds
  - optimise for stock data stream
  - realtime search engine
- Implement Circuit Breaker & Retry Pattern with RabbitMQ/Kafka
- Experiment with Hybrid Search (BM25 + Vector Search). Also personalize (feed) it.
- Benchmark Redis vs. Memcached for Caching API responses
- Implement Rate Limiting, load balancing & JWT Authentication using Nginx
- Design a Scalable Messaging Queue like WhatsApp
- Design an ultra-scalable analytics system handling petabytes of data.
- Build a Real-Time Leaderboard System with ClickHouse/Google Cloud Spanner
- Optimize Redis Streams vs. NATS vs. Apache Pulsar for real-time event streaming.
- Create a GPT-4 Powered Personal Knowledge Management System
- Design a Twitter-scale sub-10ms latency Notification System (Kafka + Redis + PostgreSQL).
- Uber surge pricing engine
- High-Performance HTTP Server (zero-copy, async, multi-threaded).
- Optimize a high-frequency trading engine (latency < 100µs).
- Rewrite a PostgreSQL query optimizer from scratch.
- a Kafka clone with exactly-once semantics

### Performance Engineering
- Optimize high-frequency trading engine for <100µs latency
- Optimize PostgreSQL query latency by 50%
- Rewrite Redis in Rust for lock-free concurrency
- Implement Redis-based vector search
- Build cryptocurrency exchange handling $1B/day volume
- Rewrite PostgreSQL's B+ Tree index for 30% faster lookups
- Write SIMD-optimized matrix multiplication in C++ (maybe AVX)
- Optimize training throughput with mixed precision
- Deploy system serving 100M users with 99.999% uptime on $500/month budget

### Algorithms & Data Structures to Master
- Graph (Dijkstra's, A*, Bellman-Ford, Tarjan, Floyd-Warshall, Prim's, Kruskal's)
- DP (Bitmask, Memoization, Tabulation)
- Tree (B, Segment, Fenwick, Van Emde Boas, Splay)
- Caching, LRU, LFU
- Trie
- Bloom Filters
- HyperLogLog
- Consistent Hashing
- Skip Lists
- Raft/paxos Consensus
- lock-free hash table using Compare-And-Swap (CAS) operations.
- Write-Ahead Logging, MVCC, log-structured merge-tree
- Optimised merge, heap, quick sort (maybe with SIMD)
- concurrent hashmap (lock-free, non-blocking).
- software transactional memory (STM) system
- BFS/DFS with manual stack/heap management to avoid recursion.

### Tools & Frameworks to Master
- PyTorch & TensorFlow
- FAISS vs Milvus vs ScaNN
- DeepSpeed & vLLM
- Kubernetes & Istio
- OpenTelemetry
- Prometheus, Grafana, Loki
- Apache Kafka
- Redis & ClickHouse
- eBPF & Flame Graphs

## Core CS Concepts
- DBMS
  - B+, R Trees
  - Query Optimization & Execution plans (also recreate the optimizer, benchmarker)
  - CAP theorem, distributed consistency
  - eventual consistency in NoSQL

### Operating Systems
- Memory Management & Virtual Memory
- Understand Latency at Scale – Memory Hierarchy, CPU Caching, Page Faults
- Process Scheduling Algorithms (custom Thread Scheduler) Build custom thread pool.
- Cache-oblivious algorithms
- Master Distributed Systems Trade-offs – CAP Theorem, PACELC, CRDTs
- I/O Performance Tuning
  - Zero-copy
  - mmap
  - io_uring
- System Calls & Kernel Optimizations

### Computer Networks
- TCP/IP Internals & Congestion Control
- WebSockets & HTTP/3
- CDN & DNS Resolution
- High-Performance DB Systems – Learn PostgreSQL internals, ClickHouse, MySQL optimizations
- Load Balancers
  - Nginx
  - Envoy

### Concurrency & Parallelism
- Thread Synchronization
  - Mutex
  - Semaphores
- Optimizing Multi-threaded Applications
- Golang Goroutines & Async Paradigms
- Lock-free Data Structures
- transactional memory

### AI for Personalization & RAG Systems
- Vector Search & Milvus Deep Dive
  - FAISS vs. HNSW vs. IVF-PQ
  - Optimizing nearest neighbor search
  - Data pipeline design for real-time vector updates
- Fine-Tuning AI Models
  - Training custom embeddings for ranking
  - Retrieval-Augmented Generation (RAG) best practices + handles realtime updates
  - Experimenting with LoRA & PEFT
- Optimizing LLM API Calls
  - Latency reduction strategies
  - Prompt engineering & prompt chaining
  - Hybrid ranking (AI + rule-based recommendations)

### Microservices Architecture
- Event-driven Architecture
  - Kafka
  - Redis Streams
- Circuit Breaker & Retry Patterns
- Service Discovery & API Gateways
- Distributed Tracing
  - Jaeger
  - OpenTelemetry

### High-Performance Databases
- Read-heavy vs. Write-heavy Optimizations
- Partitioning & Sharding Strategies
- PostgreSQL Optimization
  - VACUUM
  - ANALYZE
  - Hot-Standby
- ClickHouse Internals & Use Cases

### Scalability & Load Management
- Horizontal vs. Vertical Scaling Strategies
- Redis for Caching
  - Write-behind
  - Cache Stampede Prevention
- Queue-based Load Distribution
  - RabbitMQ
  - Kafka
- Optimizing Socket-based Real-time Systems

### Security Best Practices
- OAuth 2.0 & JWT Security Models
- Secure API Development
  - HSTS
  - CSPf
  - CSRF
- Preventing SQL Injection & XSS
- Secrets Management
  - HashiCorp Vault
  - AWS KMS

### Cloud Infrastructure
- AWS/GCP Deep Dive
  - Cost-efficient Autoscaling Strategies
  - Serverless with AWS Lambda, scaling it with kubernetes, optimise cold start times. (provisioned concurrency & Snapstart)
  - Auto-healing, Service Mesh, Envoy Proxy
  - Step Functions
  - spot instances
- Kubernetes & Helm for Microservices
- Infrastructure as Code
  - Terraform
  - CloudFormation

### Observability & Reliability
- Distributed Tracing
  - Jaeger
  - Zipkin
- Logging Best Practices
  - ELK Stack
  - Promtail
- Prometheus & Grafana for Monitoring & Alerting
- SLO/SLI Implementation

### Performance Engineering
- Load Testing
  - k6
  - Locust
- Profiling Backend Services
  - pprof
  - perf
  - Valgrind
- FSDP
- Database Tuning
  - Connection Pool Strategies
  - Query Optimization
  - Index Design

## 💡 Strategic Technical Innovation

### Building AI-first Products
- Multi-modal AI Integration
  - Text + Vision Processing
  - Speech Recognition
- AI-driven Analytics Dashboards
- Adaptive Learning Systems
  - Personalization Engines
  - Recommendation Systems
  - speech-to-text pipeline (whisper)

### Monetization & Growth Engineering
- Payment Gateway Optimization
  - Stripe Integration
  - Fraud Detection
- Gamification Strategies
  - User Engagement
  - Retention Mechanics
- SEO for AI-generated Content
  - Content Strategy
  - Performance Metrics

### Web3 & Edge Computing (Optional)
- Smart Contract Development
  - Solidity
  - Web3.js
- AI-powered Blockchain Analytics
- Edge Computing for Real-time AI
  - TensorFlow Lite
  - ONNX Runtime