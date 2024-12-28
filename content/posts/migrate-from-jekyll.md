---
author:
  name: "Deekshitha Reddy"
date: 2024-07-05
linktitle: Memory Design 
title: Memory Design
type:
- post
- posts
weight: 10
series:
- Hugo 101
---


## Lets design the memory!

This post explains a simple cache-memory architecture designed for an RV64I-based system. The architecture consists of three modules: `l1_cache`, `main_memory`, and `cache_controller`. Each module and its functionality is outlined below.

---

### **1. L1 Cache**
The `l1_cache` module represents the Level-1 (L1) cache. It provides faster access to frequently used data compared to main memory.

#### **Inputs and Outputs**
- **Inputs**:
  - `clk`: Clock signal.
  - `mode`: Operation type (read: `0`, write: `1`).
  - `address`: A 64-bit memory address.
  - `write_data`: 64-bit data to be written into the cache.
- **Outputs**:
  - `read_data`: 64-bit data read from the cache.
  - `hit`: Indicates whether the requested data is found in the cache.
  - `stored_data` and `stored_address`: Represent the data and address evicted from the cache during a write.

#### **Cache Structure**
- The cache is modeled as a memory array (`l1_cache`) with **64 blocks** of 64 bytes (512 bits) each. The 64 bit address is divided into: 
  - **Tag**
  - **Block**
  - **Word Offset**
-This cache will be a simple directly mapped cache.
- A **valid bit array** tracks whether the data in each cache block is valid.

#### **Read and Write Operations**
1. **Write Operation (`mode == 1`)**:
   - If the target block is invalid, the new tag and data are written to the cache.
   - If the block is valid but being overwritten, the existing data and address are evicted and replaced.
   - The specific word in the block to update is determined by the `Word Offset` in the address.

2. **Read Operation (`mode == 0`)**:
   - On a cache **hit**, the corresponding word is read from the cache.
   - On a **miss**, the data is fetched from main memory.

---

### **2. Main Memory**
The `main_memory` module models a larger, slower memory hierarchy level. It has the following features:

#### **Structure**
- **Memory Size**: 4GB (2^32 bytes) divided into **64-byte blocks**.
- **Data**: Each block contains 8 words of 64 bits each.

#### **Inputs and Outputs**
- Inputs: `clk`, `mode`, `address`, `write_data`, and `hit`.
- Output: `read_data` provides the requested 64-bit word.

#### **Functionality**
- Reads data from the block indexed by the upper address bits.
- Writes data to the specified word in the block if there’s a cache miss.

---

### **3. Cache Controller**
The `cache_controller` module coordinates the interaction between `l1_cache` and `main_memory`. It:
- Instantiates both the `l1_cache` and `main_memory` modules.
- Ensures seamless data transfer during cache eviction by passing evicted data (`stored_data` and `stored_address`) from the cache to main memory.
- Directs read and write operations to the appropriate memory level based on the `hit` signal.

---

### **Summary**
This architecture models a simple RV64I cache-memory hierarchy. The `l1_cache` ensures faster data access for frequently used data, while the `main_memory` provides larger, slower storage. The `cache_controller` facilitates efficient data management between these levels. This design highlights concepts like cache hits, misses, eviction policies, and tag-based indexing, which are foundational to modern CPU architectures.

