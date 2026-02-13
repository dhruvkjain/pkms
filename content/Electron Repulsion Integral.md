**The Problem:** The bottleneck of Hartree-Fock (HF) and DFT calculations is the evaluation of 4-center 2-electron repulsion integrals (ERIs). Standard open-source tools (like PySCF’s `libcint`) use C code that is often hard to parallelize safely or lacks modern AVX-512 optimizations.

- **The Standard (PySCF / `libcint`):**
    - **Method:** Pure Analytic Integrals.
    - **Scaling:** Formally $O(N^4)$, where $N$ is the number of basis functions.
    - **Bottleneck:** Calculating millions of "small" integrals that eventually sum to zero.
    - **Language:** C (unsafe pointer arithmetic) wrapped in Python.
        
- **The Schrödinger Way (Jaguar):**
    - **Method:** **Pseudospectral (PS)**.
    - **Scaling:** $O(N^{2.3})$ (scaling improves as systems get larger).
    - **The Trick:** Instead of calculating every integral analytically, Jaguar projects the electron density onto a **physical 3D grid** to perform the Coulomb ($J$) and Exchange ($K$) sums as simple vector dot-products.
    - **The Role of Analytic Integrals:** Jaguar _still_ needs a blazing fast analytic engine to calculate the **"Grid Correction"** (the difference between the perfect analytic value and the grid approximation).

**The Solution:** **Rust-ERI** is a pure Rust library that computes ERIs using the **Obara-Saika (OS)** and **McMurchie-Davidson (MD)** recursion schemes. It features:
- **SIMD-First Design:** Computes integrals in batches of 8 or 16 (f64) using `wide` crate.
- **Jaguar Alignment:** Designed to act as the "Analytic Correction" engine in a Pseudospectral workflow.
- **Safety:** Uses Rust’s borrow checker to manage the massive memory arenas required for recursion without segfaults.

To maximize SIMD throughput, we reject the standard "Array of Structs" (AoS) pattern used in C libraries. Instead, we use **Structure of Arrays (SoA)** to ensure contiguous memory loads for AVX-512 vectors.

- **Target:** Outperform standard `libcint` (used by PySCF) in single-thread throughput for contracted basis sets.
- **Strategy:** Implement the **Head-Gordon/Pople (HGP)** algorithm, a hybrid of Obara-Saika (OS) and McMurchie-Davidson (MD), specifically optimized for AVX-512 vectorization.
- **Memory Model:** Arena-based allocation for recursion stacks to prevent heap fragmentation during millions of small integral evaluations.


```text
rust-eri/
├── Cargo.toml                # Dependencies: rayon, serde, serde_json, portable_simd
├── benches/                  # Criterion benchmarks
│   └── benchmark_eri.rs      # The "Water Cluster" & "S66" benchmark runner
├── data/                     # Your XYZ files and Basis Sets
│   ├── water_50.xyz
│   ├── s66_dimer.xyz
│   └── cc-pvdz.json          # Basis set definitions
├── examples/
│   └── simple_energy.rs      # "Hello World": Calculates energy of H2
├── src/
│   ├── lib.rs                # Public API exports
│   ├── basis/                # parsing and organizing data
│   │   ├── mod.rs
│   │   ├── parser.rs         # Reads JSON basis sets (300 LOC)
│   │   └── structs.rs        # Defines Shell, Primitive, Atom structs
│   ├── engine/               # The "HGP" Algorithm (The Core)
│   │   ├── mod.rs
│   │   ├── vrr.rs            # Vertical Recurrence (Obara-Saika) (500 LOC)
│   │   ├── hrr.rs            # Horizontal Recurrence (Head-Gordon/Pople) (400 LOC)
│   │   ├── contract.rs       # Loops over primitives to build shells (300 LOC)
│   │   └── screen.rs         # Cauchy-Schwarz Screening logic (100 LOC)
│   └── math/                 # Low-level Math & SIMD
│       ├── mod.rs
│       ├── simd.rs           # Wrappers for AVX-512 (std::simd) (200 LOC)
│       └── special.rs        # Fast Error Function (erf) & Gamma function
└── tests/                    # Correctness tests
    └── verify_pyscf.rs       # Compares your numbers vs PySCF output
```