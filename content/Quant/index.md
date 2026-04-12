---
title: Quant
---
[[index|Home]]


- History of Market and trading: https://youtu.be/DLg5tyxmXqQ?si=kqaQc9xvFzJwk_r4
- [Varsity by Zerodha](https://zerodha.com/varsity/)
- [Goldman Sach Hackathon - 2024](https://www.hackerrank.com/contests/gs-india-hackathon-2024/challenges/quant-option-pricing-model)
- [Why and How doing nothing is better](https://youtu.be/TQuxVz52w2w?si=aUkICeeeGtiVUCHR)
- [The Sharpe Ratio Explained (by a quant trader)](https://youtu.be/9HD6xo2iO1g?si=0gBU2Z3dJ9_5dOxo)
- [Coding Jesus Videos - Youtube](https://www.youtube.com/@CodingJesus/videos)
- [TradeMath - leetcode for Quant](https://www.tradermath.org/brainteasers)

## Conf videos on youtube
- [The Making of an Ultra Low Latency Trading System with Go and Java](https://youtu.be/6SXd0cNRVN8?si=Iig40l0rQRfsCo0n)
- 

## HRT
- [HRT's tech blogs](https://www.hudsonrivertrading.com/hrtbeat/)


## IMC
IMC Lauchpad Program: https://www.imc.com/in/careers/students-graduates/programs/launchpad

- [IMC's Trading Masterclass 2024 - Youtube](https://youtu.be/DgowLNC4X_k?si=3mmKQphb2TWcVAQV)
- [IMC's Quantitative Research Masterclass 2025 - Youtube](https://youtu.be/BY6Ug2QTQrU?si=SwcWNwpZFou_rAsG)


Meetup:
- [The evolving search for modern API's in Boost](https://youtu.be/Pglsk818dUk?si=WlQK6MSJdTnT3EkW)
- [Conan C++ package manager](https://youtu.be/CCKiKGn-ew8?si=iVg1XiDGeEnjo4EZ)
- [A story of one Exception](https://youtu.be/cWHO3KXcdhU?si=OsJrHFBGFKSDUxjs)
- [The Curious symbiosis between C++ and the Linker](https://youtu.be/twNoIhGKIoo?si=1d8ETTaOcPCfbZZw)
- [Going NATS](https://youtu.be/wMWd78aZhxI?si=tn3EBlJgtGcwMX--)


## Books
- [A Practicle guide to Quantitative finance Interviews by Xinfeng zhou](https://academyflex.com/wp-content/uploads/2024/03/a-practical-guide-to-quantitative-finance-interviews.pdf)

## Research Papers/Articles:
- Apr 2019: [ABIDES: TOWARDS HIGH-FIDELITY MARKET SIMULATION FOR AI RESEARCH](https://arxiv.org/pdf/1904.12066)
- Sep 2023: [C++ design patterns for low-latency applications including high-frequency trading](https://arxiv.org/pdf/2309.04259)
- [Quant firms types and other relevant details](https://fessorpro.com/blog/algo_firms)
- HRT Summer Interns projects: https://www.hudsonrivertrading.com/hrtbeat/intern-spotlight-software-engineering-summer-projects/
	- https://www.hudsonrivertrading.com/hrtbeat/intern-spotlight-hrt-ai-labs-project/
		- The empirical work presented in the blogpost is very interesting (congrats to all involved). I want to add to the discussion by mentioning that diffusion models will always under-characterize tail probabilities when your target (e.g. equity returns) is heavy-tailed. This is a mathematical result proven in our recent paper (Tam and Dunson [https://arxiv.org/abs/2501.07763](https://arxiv.org/abs/2501.07763))


## Basics GK 
Data are of two types Quantitative and Qualitative 
- **Quantitative** => you can measure => Finance is all about numbers => Quant Finance => Data-driven, analytical, model-based
		Important considering factor for Venture capitals, Investors etc.
- **Qualitative** =>  subjective factors like management quality, brand value, regulatory risks, and macroeconomic trends
		Important considering factor for Private equity, Risk management, HFT, asset pricing etc.



## **Foundation (CPU/Hardware)**

1. CPU cache hierarchy (L1/L2/L3)
2. Cache lines and alignment
3. Cache coherency (MESI/MOESI)
4. False sharing
5. True sharing and cache bouncing
6. Memory barriers and fences
7. Store buffers and load buffers
8. Write-combining
9. Branch prediction (static/dynamic)
10. Instruction pipelining
11. Out-of-order execution
12. Speculative execution
13. SIMD basics (SSE, AVX, NEON)
14. Prefetching (hardware/software)
15. TLB (Translation Lookaside Buffer)
16. Page faults and memory paging
17. NUMA topology and awareness
18. CPU affinity and core pinning
19. Hyper-threading effects

## **Memory & Data Structures**

20. Memory layout and padding
21. Struct packing and alignment
22. Pointer aliasing
23. Temporal/spatial locality
24. Working set size
25. Cache line coloring
26. Ring buffers
27. Circular queues
28. Lock-free queues (MPMC, SPSC)
29. Skip lists (lock-free variants)
30. Hash tables (concurrent)
31. Memory pools and pre-allocation
32. Object reuse patterns
33. Disruptor pattern
34. Bounded queues vs unbounded

## **Synchronization & Concurrency**

35. Atomics (compare-and-swap, load/store)
36. Memory ordering (acquire/release/relaxed)
37. Spinlocks vs mutexes
38. Reader-writer locks
39. Condition variables
40. Semaphores
41. Lock-free vs wait-free
42. ABA problem
43. Acquire/release semantics
44. Sequential consistency
45. Double-checked locking
46. Seqlock pattern

## **Threading & Scheduling**

47. Thread creation overhead
48. Thread pooling
49. Work stealing
50. Thread affinity pinning
51. NUMA-aware scheduling
52. Real-time scheduling (SCHED_FIFO/SCHED_RR)
53. CPU isolation (isolcpus)
54. Context switching costs
55. Jitter sources
56. Priority inversion
57. Busy-wait loops
58. Backpressure strategies

## **I/O & Networking**

59. Polling vs interrupts
60. Interrupt coalescing
61. Socket options (TCP_NODELAY, SO_REUSEADDR)
62. UDP vs TCP tradeoffs
63. Non-blocking I/O
64. epoll/kqueue/IOCP
65. Event-driven architecture
66. Reactor pattern
67. Proactor pattern
68. Zero-copy (sendfile, mmap, io_uring)
69. RX/TX ring buffers (NIC level)
70. DPDK basics
71. Kernel bypass
72. Packet filtering/XDP

## **System Calls & OS Interaction**

73. Syscall overhead
74. Avoiding syscalls
75. Batch syscalls
76. vDSO (virtual dynamic shared object)
77. Read/write/mmap syscalls
78. mlock/mlockall
79. Process vs thread creation
80. Signal handling in low-latency code

## **Profiling & Analysis Tools**

81. perf (Linux performance counters)
82. Flamegraphs
83. VTune (Intel)
84. Cachegrind/Callgrind
85. Strace syscall tracing
86. ltrace library tracing
87. GDB debugging
88. Valgrind/Helgrind
89. ThreadSanitizer
90. CPU cycle counting (RDTSC)
91. Microbenchmarking frameworks
92. Statistical significance in benchmarks
93. Variance and tail latency analysis
94. Load testing tools

## **C++ Specific**

95. Move semantics and RVO
96. RAII and scope guards
97. Smart pointers (unique/shared)
98. Noexcept and exception safety
99. Constexpr and compile-time computation
100. Templates and specialization
101. SFINAE and concepts
102. Variadic templates
103. Inline assembly (**asm**)
104. Compiler intrinsics (__builtin_*)
105. Volatile and compiler barriers
106. Undefined behavior pitfalls
107. Aliasing rules
108. Link-time optimization (LTO)
109. Profile-guided optimization (PGO)

## **Rust Specific**

110. Ownership and borrowing semantics
111. Send/Sync traits
112. Interior mutability (Cell/RefCell/Atomic)
113. Async/await for systems
114. Tokio/async-std runtimes
115. Crossbeam (threading/concurrency)
116. Parking lot mutexes
117. Unsafe code and correctness
118. Inline assembly in Rust
119. SIMD in Rust (packed_simd, std::simd)

## **Design Patterns & Architecture**

120. Ring buffer patterns
121. Event sourcing
122. Command pattern
123. Observer pattern
124. Pub-sub architectures
125. Pipeline patterns
126. Batching for throughput
127. Backpressure handling
128. Graceful degradation
129. Circuit breakers
130. Rate limiting

## **Benchmarking & Validation**

131. Microbenchmarking methodology
132. Cold vs warm cache testing
133. Variance measurement
134. Reproducibility
135. Tail latency percentiles
136. Throughput vs latency tradeoffs
137. Load generation
138. Stress testing
139. Long-running stability tests
140. Regression testing

## **Real-world HFT/Low-Latency Specifics**

141. Order matching engines
142. Market data processing
143. Serialization formats (FIX, protobuf, flatbuffers)
144. Time synchronization (NTP, PTP)
145. Latency budgets
146. Jitter quantification
147. Tick-to-trade timing
148. Colocation considerations
149. Network latency measurement
150. Hardware clocking

## **Build & Deployment**

151. CMake/Bazel build optimization
152. Compiler flags for performance
153. Static vs dynamic linking
154. Code generation options (-O3, -march, etc)
155. Stripping/dead code elimination
156. Docker for reproducibility
157. Linux container tuning
158. Kernel parameters (sysctl)

