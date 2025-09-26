# Parallel Computing Project

## Overview

This project implements a parallel computing solution for finding similarities between datasets using the Jaccard Index algorithm. The implementation uses MPI (Message Passing Interface) to distribute computational tasks across multiple processes, enabling efficient analysis of large datasets.

## Problem Description

Finding similarities between datasets involves analyzing the degree of resemblance or correspondence between different sets of data. This is a crucial task in various fields including data integration, data cleaning, data matching, machine learning, and data exploration. The goal is to identify common patterns, relationships, or entities within datasets to derive meaningful insights.

## Algorithm: Jaccard Index

The Jaccard Index quantifies the overlap between two sets by calculating the ratio of the size of their intersection to the size of their union:

```
J(A, B) = |A ∩ B| / |A ∪ B|
```

Where:
- `|A ∩ B|` represents the size of the intersection of sets A and B
- `|A ∪ B|` represents the size of their union

The resulting Jaccard Index ranges from 0 to 1:
- **0**: No common elements (maximum dissimilarity)
- **1**: Identical sets (maximum similarity)

## Technology Stack

- **Programming Language**: C (C99 specification)
- **IDE**: CLion
- **Build System**: CMake
- **Parallel Computing**: MPI library (`<mpi.h>`)

## Features

- **Serial Algorithm**: Sequential calculation of Jaccard index for dataset pairs
- **Parallel Algorithm**: Distributed processing using MPI for improved performance
- **Data Normalization**: Automatic scaling of input data to [0,100] range
- **Multiple Format Support**: UTF-8 text files with comma-separated values
- **Comprehensive Testing**: Performance analysis with various dataset sizes
### Prerequisites

- C compiler with C99 support
- MPI library (MPICH or OpenMPI)
- CMake (version 3.0 or higher)
- Bash shell (for automation scripts)

### Quick Start with Bash Scripts

The project includes several bash scripts to automate common tasks:

1. **Clone the repository:**
```bash
git clone https://gitlab.com/semen.mokrov.ozu/parallel_computing.git
cd parallel_computing
```

2. **Build the project:**
```bash
./build.sh
```

3. **Generate test data:**
```bash
./generate_data.sh
```

4. **Run the program:**
```bash
./run.sh -n 4 2 resources/1/1.txt 1000000 resources/2/1.txt 1000000
```

Execute the program using MPI with the following command:

```bash
mpiexec -n <number_of_processes> ./cmake-build-release/parallel_computing_project.exe
```

## Usage

### Input Format

The program expects:
1. Number of datasets
2. Pathname for each dataset file
3. Number of values in each dataset

### Dataset Format

- Files must contain comma-separated values
- Values are automatically normalized to [0,100] range
- Supported formats: Any UTF-8 text file

### Example Input

```
3
resources/1/1.txt
resources/2/1.txt
resources/3/1.txt
1000000
1000000
1000000
```

## Performance Analysis

### Testing Results

The project includes comprehensive performance testing with datasets ranging from 100,000 to 10,000,000 elements using 1, 3, 6, 8, 10, and 12 processes.

#### Key Findings:

1. **Scalability**: Performance improves with increasing number of processes for larger datasets
2. **Overhead**: Small datasets may show decreased performance with more processes due to MPI overhead
3. **Optimal Range**: Best performance observed with 8-12 processes for large datasets

#### Sample Performance Data:

| Dataset Size | 1 Process | 3 Processes | 6 Processes | 8 Processes | 10 Processes | 12 Processes |
|--------------|-----------|-------------|-------------|-------------|--------------|--------------|
| 1M elements  | 1190ms    | 905ms       | 887ms       | 575ms       | 392ms        | 263ms        |
| 2M elements  | 1180ms    | 942ms       | 917ms       | 646ms       | 425ms        | 283ms        |
| 5M elements  | 1252ms    | 1017ms      | 1191ms      | 733ms       | 503ms        | 349ms        |
| 10M elements | 1261ms    | 1200ms      | 1170ms      | 894ms       | 691ms        | 559ms        |

## Project Structure

```
parallel-computing-project/
├── CMakeLists.txt          # CMake configuration
├── main.c                  # Main program source
├── report.pdf              # Detailed project report
├── resources/              # Test datasets
│   ├── 1/                 # Dataset 1 files
│   ├── 2/                 # Dataset 2 files
│   └── ...                # Additional datasets
├── res/                   # SLURM job outputs
└── README.md              # This file
```

## Algorithm Implementation

### Serial Algorithm
1. Count frequency of each element (0-100) in each dataset
2. Compare frequency distributions for all dataset pairs
3. Calculate Jaccard index using intersection and union

### Parallel Algorithm
1. Distribute dataset processing across MPI processes using `MPI_Scatter`
2. Each process counts frequencies for assigned data portions
3. Gather results using `MPI_Gather`
4. Broadcast results to all processes using `MPI_Bcast`
5. Calculate similarity indices

## Future Improvements

1. **Pair-wise Parallelization**: Parallelize the comparison of dataset pairs
2. **Topological Networks**: Implement hypercube or ring topologies for better process communication
3. **Process Optimization**: Limit processes to even numbers matching dataset sizes
4. **Memory Optimization**: Implement more efficient data structures for large datasets

## Testing

The project includes comprehensive testing with:
- Small datasets (100K-1M elements) for algorithm validation
- Large datasets (1M-10M elements) for performance analysis
- Mixed dataset sizes for robustness testing
- Supercomputer testing on high-performance computing clusters

## Documentation

For detailed technical documentation, algorithm analysis, and comprehensive test results, please refer to:

**[📄 Project Report (PDF)](report.pdf)**

## References

- [Analytics Vidhya - String Similarity Metrics](https://www.analyticsvidhya.com/blog/2021/02/a-simple-guide-to-metrics-for-calculating-string-similarity/)
- [Ozyegin University LMS](https://lms.ozyegin.edu.tr/course/view.php?id=4291)
- [MPI Forum Documentation](https://www.mpi-forum.org/docs/)
- [MPICH Documentation](https://www.mpich.org/static/docs/latest/)
- [CU Research Computing - MPI C Guide](https://curc.readthedocs.io/en/latest/programming/MPI-C.html)