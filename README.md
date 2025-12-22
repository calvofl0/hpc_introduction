# Foundations of High-Performance Computing (HPC)

A comprehensive LaTeX Beamer presentation for an introduction to High-Performance Computing course.

## Overview

This presentation provides a detailed academic introduction to HPC concepts, designed for a 4-6 hour course. The content is programming language-agnostic and tailored for a broad audience with basic Linux command-line knowledge.

## Contents

### 1. Demystifying HPC Hardware
- CPU architecture basics (cores, sockets, threads)
- Memory hierarchy and cache levels
- Shared vs distributed memory systems
- HPC cluster architecture
- SLURM terminology: CPUs vs tasks
- Visual diagrams of hardware layouts

### 2. Understanding Parallelism
- Why parallel computing?
- Levels of parallelism:
  - Instruction-level parallelism (ILP)
  - Vectorization (SIMD)
  - Thread-level parallelism (shared memory)
  - Process-level parallelism (distributed memory)
- Multi-threading with OpenMP
- Message passing with MPI
- Hybrid MPI+OpenMP models
- Amdahl's Law and speedup limitations

### 3. Cluster Workflow and SLURM Basics
- Introduction to SLURM workload manager
- Common SLURM commands
- Anatomy of SLURM job scripts
- Example job scripts:
  - Serial jobs
  - Multi-threaded (OpenMP) jobs
  - Distributed (MPI) jobs
  - Hybrid MPI+OpenMP jobs
- Job arrays for parameter sweeps
- Interactive jobs with srun and salloc

### 4. Translating Concepts into Projects
- Programming language support (Python, Julia, R, C/C++)
- Python parallel computing:
  - Threading vs multiprocessing
  - MPI with mpi4py
- Julia native parallelism
- R parallel packages
- Debugging strategies
- Performance optimization tips
- Scaling studies (strong vs weak scaling)

### 5. Q&A and Interaction
- Practical exercises:
  - Writing SLURM scripts
  - Parallel Python programs
  - Job arrays
- Common pitfalls and solutions
- Resource estimation guidelines
- Best practices summary
- Additional learning resources

## Features

- **Visual Diagrams**: TikZ-based diagrams illustrating hardware architecture, memory hierarchy, parallelism models, and resource allocation
- **Code Examples**: Practical examples in multiple programming languages with SLURM integration
- **Interactive Exercises**: Hands-on activities for learning SLURM and parallel programming
- **UNIL Theme**: Professional styling using the UNIL Beamer theme

## Requirements

To compile the presentation, you need:

- LaTeX distribution (e.g., TeX Live, MiKTeX)
- Required packages:
  - beamer
  - tikz
  - listings
  - pgfplots
  - booktabs
  - amsmath, amssymb
- UNIL Beamer theme (included: `beamerthemeUNIL.sty`)
- Logo files: `logo.png`, `logo.svg`

## Compilation

To compile the presentation:

```bash
pdflatex hpc_foundations.tex
pdflatex hpc_foundations.tex  # Run twice for TOC and references
```

Or using latexmk:

```bash
latexmk -pdf hpc_foundations.tex
```

## File Structure

```
.
├── hpc_foundations.tex        # Main presentation file
├── beamerthemeUNIL.sty       # UNIL Beamer theme
├── logo.png                   # UNIL logo (PNG format)
├── logo.svg                   # UNIL logo (SVG format)
└── README.md                  # This file
```

## Customization

You can customize the presentation by:

1. **Updating contact information**: Edit the last slide to add your contact details and HPC support information
2. **Adjusting content**: Each section is clearly marked with LaTeX section comments
3. **Modifying examples**: Update SLURM job scripts with your cluster-specific module names and configurations
4. **Adding slides**: Insert new frames within the appropriate sections

## Target Audience

- Researchers new to HPC
- Graduate students beginning computational work
- Anyone with basic Linux command-line knowledge
- Users needing to understand SLURM job submission

## Learning Objectives

After completing this course, participants will be able to:

1. Understand HPC hardware architecture and terminology
2. Explain different levels and models of parallelism
3. Write and submit SLURM job scripts for various parallel computing scenarios
4. Choose appropriate parallelization strategies for their projects
5. Use parallel programming tools in Python, Julia, or R
6. Debug and optimize parallel programs
7. Conduct scaling studies to evaluate parallel performance

## License

This presentation is created for educational purposes in HPC training.

## Acknowledgments

- UNIL (University of Lausanne) for the Beamer theme
- The HPC community for best practices and examples
