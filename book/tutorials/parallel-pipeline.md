(parallel-tutorial)=
# Using parallel Pipeline execution

QIIME 2 provides formal support for parallel computing of {term}`Pipelines <pipeline>` through [Parsl](https://parsl.readthedocs.io/en/stable/1-parsl-introduction.html>).[^formal-informal-parallel]
This allows for faster execution of QIIME 2 `Pipelines`, assuming the compute resources are available, by ensuring that pipeline steps that can run simultaneously do run simultaneously.

Parallel Pipeline execution is accessible in different ways depending on which interface you're using.
Here we illustrate how to run `Pipelines` in parallel using {term}`q2cli` and {term}`QIIME 2's Python 3 API <Python 3 API>`.

```{note}
These examples assume that you have a QIIME 2 deployment that includes the [q2-dwq2](https://github.com/caporaso-lab/q2-dwq2) educational plugin.
```

## q2cli

Review the help text for a QIIME 2 `Pipeline`.
Pay special attention to the usage examples at the bottom of the help text.

```shell
qiime dwq2 search-and-summarize --help
```

Have QIIME 2 generate example data that can be used to run the usage example.

```shell
qiime dwq2 search-and-summarize --example-data
```

Run the usage example serially first.
Note that in the following commands the output filenames are adapted from the usage example to prepend `serial-` to each file name.

```shell
qiime dwq2 search-and-summarize \
    --i-query-seqs query-seqs.qza \
    --i-reference-seqs reference-seqs.qza \
    --m-reference-metadata-file reference-metadata.qza \
    --p-split-size 1 \
    --o-hits serial-hits.qza \
    --o-hits-table serial-hits-table.qzv
```

To run this `Pipeline` in parallel, append the `--parallel` flag, which will run this command in parallel using a default parallel configuration.
Note that the output filenames this time are adapted to prepend `parallel-` to each file name.

```shell
qiime dwq2 search-and-summarize \
    --i-query-seqs query-seqs.qza \
    --i-reference-seqs reference-seqs.qza \
    --m-reference-metadata-file reference-metadata.qza \
    --p-split-size 1 \
    --o-hits parallel-hits.qza \
    --o-hits-table parallel-hits-table.qzv \
    --parallel
```

If you're using a system with parallel computing capabilities (e.g., at least six cores) the parallel execution of this command should have run faster than the serial execution.

## Python 3 API

Lorem ipsum yada yada yada...

[^formal-informal-parallel]: QIIME 2 {term}`Actions <Action>` can provide formal (i.e., Parsl-based) or informal (e.g., multi-threaded execution of a third party program) parallel computing support.
 To learn more about the distinction, see [](types-of-parallel-support).