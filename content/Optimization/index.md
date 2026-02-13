[[index|Home]]


[LeetGPU - leetcode for optimizations](https://leetgpu.com/)

### Rust optimizations / Advance Rust
- [Measure Rust Performance - Let's Get Rusty YT](https://youtu.be/q3VOsGzkM-M) 
- [Advance Rust Techniques - GOTO conf](https://youtu.be/QQzAWxYKPSE)

### SIMD (Single Instruction Multiple Data)
[https://linebender.org/blog/towards-fearless-simd/]

[The Future of SIMD, with Raph Levien](https://youtu.be/y0WcCUKxk50?si=XxrXi_L8rA_HNOlr)

SIMD = Single Instruction Multiple Data 

In GPU we have SIMT i.e. Single Instruction Multiple Threads (32 threads per wrap/group), where each thread can access same or different data

let's say we have a if statement if A else B then, a SIMT compiler will map that to a predication/predicate mask(a bitmask or a set of boolean values). Each value of predication mask represents a case where condition is true or false according to the boolean value it holds.
`true` or `1` means operation proceeds
`false` or `0` means operation is skipped

### Layout biases measurement
- ["Performance Matters" by Emery Berger](https://youtu.be/r-TLSBdHe1A)