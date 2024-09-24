(types-of-parallel-support)=
# Types of parallel computing support

## Formal parallel support

QIIME 2's formal parallel computing support uses [Parsl](https://parsl.readthedocs.io/en/stable/1-parsl-introduction.html>), and is only accessible through qiime2 {term}`Pipeline` actions.
All QIIME 2 `Pipelines` will have parallel computing options (e.g., the `--parallel` parameter in {term}`q2cli`), though whether those actually induce parallel computing is up to the implementation of the `Pipeline`.
Actions using this formal parallel computing support can make use of high-performance computing hardware that doesn't necessarily have shared memory.

## Informal parallel support

Some {term}`Method` actions (e.g., `qiime dada2 denoise-*`) wrap multi-threaded applications and may define a parameter (like `--p-n`) that gives the user control over that.
The QIIME 2 parameter type associated with these parameters should always be `NTHREADS` or `NJOBS` (if you observe a parameter where this isn't the case, it was probably an error on the developers part - reach out on the forum to let us know).
Actions using this informal parallel computing support are generally restricted to running on systems with shared memory.