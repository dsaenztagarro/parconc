Number of processors
sysctl -n hw.ncpu

### The Eval Monad, rpar, and rseq

```haskell
-- compile with full Optimisation (-O2)
cabal build -O2 rpar
-- tell GHC runtime system (+RTS) to use two cores (-N2)
cabal run rpar 1 +RTS -N2
cabal run rpar 2 +RTS -N2
cabal run rpar 3 +RTS -N2
cabal run rpar 4 +RTS -N2
-- alternatively, to force Optimisation
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

```haskell
cabal run sudoku1 testdata/sudoku17.1000.txt
1000
-- (+RTS -s) instructs the GHC runtime system to emit the statistics shown.
cabal run sudoku1 testdata/sudoku17.1000.txt +RTS -s
```
