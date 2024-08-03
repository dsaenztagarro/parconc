Number of processors

```bash
# Number of processors
sysctl -n hw.ncpu

# ThreadScope
# - Download binary to Packages folder:
#   https://github.com/haskell/ThreadScope/releases
#   chmod +x threadscope.macOS-latest.ghc-9.2.2
# - brew install gtk+
```

### The Eval Monad, rpar, and rseq

```bash
# compile with full Optimisation (-O2)
cabal build -O2 rpar
# tell GHC runtime system (+RTS) to use two cores (-N2)
cabal run rpar 1 +RTS -N2
cabal run rpar 2 +RTS -N2
cabal run rpar 3 +RTS -N2
cabal run rpar 4 +RTS -N2
# alternatively, to force Optimisation
cabal run rpar 4 -O2 +RTS -N2
```

```haskell
{-
evaluates to WHNF
The rule of thumb is to use evaluate to force or handle exceptions in lazy values.

evaluate (error "foo") >> error "bar"

is guaranteed to throw "foo".
-}
evaluate :: a -> IO a
```

### Example: Parallelizing a Sudoku Solver

```bash
cabal run sudoku1 testdata/sudoku17.1000.txt
1000
# (+RTS -s) instructs the GHC runtime system to emit the statistics shown.
cabal run sudoku1 testdata/sudoku17.1000.txt +RTS -s
```

-eventlog is deprecated: the eventlog is now enabled in all runtime system ways

```cabal
-- parconc.cabal
executable sudoku2:
  ghc-options: -threaded -eventlog
```

```bash
# (+RTS -l) generates eventlog, to be loaded in threadscope
cabal run sudoku2 testdata/sudoku17.1000.txt +RTS -N2 -l
# => sudoku2.eventlog
# threadscope sudoku.eventlog
```

Amdahl's law

### Example: The K-means problem

```bash
cabal run GenSamples -O2
# => clusters
# => params
# => points
# => points.bin
cabal run kmeans seq -O2
# (+RTS -lf) generates spark events in eventlog
cabal run kmeans strat 64 -O2 -- +RTS -N4 -lf
```
