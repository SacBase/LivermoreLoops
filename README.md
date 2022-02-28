# LivermoreLoops
Sac implementations of the Livermore Loop benchmarks

Building programs require operational sac2c and sac4c as well as CMake at least version 3.3.

Building instructions:
```bash
$ cd LivermoreLoops
$ git submodule init
$ git submodule update
$ mkdir build
$ cd build
$ cmake ..
$ make -j5
$ ctest -j N
```
Compiled files will be available in the `build` directory, under corresponding
sub-directories.  Each directory will be built for `seq` and `mt_pth` targets,
appending the name of the target to the name of the directory.

Performance and unit tests are available with ctest. The value N
is the number of threads ctest will execute concurrently.
Unlike make -j, N must be specified, or its value is assumed
to be 1.

## Benchmarks

- Livermore Loop # 1: (Parallel) hydro fragment
- Livermore Loop # 2: (Sequential) incomplete Cholesky - CG
- Livermore Loop # 3: Inner product vector-vector
- Livermore Loop # 4: Banded linear equations
- Livermore Loop # 5: Tri-diagonal elimination, below diagonal
- Livermore Loop # 6: General linear recurrence equations
- Livermore Loop # 7: Equation of state fragment
- Livermore Loop # 8: A.D.I. integration

