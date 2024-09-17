(artifacts-as-metadata)=
# How to use QIIME 2 Artifacts as Metadata

In addition to TSV metadata files, QIIME 2 also supports viewing some kinds of artifacts as metadata.
An example of this is artifacts of type ``SampleData[AlphaDiversity]``.

To get started with understanding artifacts as metadata, first download an example artifact:

```shell
curl -sL \
  "https://data.qiime2.org/2021.4/tutorials/metadata/faith_pd_vector.qza" > \
  "faith_pd_vector.qza"
```

To view this artifact as metadata, simply pass it in to any method or visualizer that expects to see metadata (e.g. ``metadata tabulate`` or ``emperor plot``):

```shell
qiime metadata tabulate \
    --m-input-file faith_pd_vector.qza \
    --o-visualization tabulated-faith-pd-metadata.qzv
```

```{admonition} Exercise
What is the largest value of Faith's PD?
What is the smallest? Hint: use the column sorting functions to assist with this query.
```

When an artifact is viewed as metadata, the result includes that artifact's provenance in addition to its own.

```{admonition} Exercise
Identify this artifact in the interactive provenance graph after loading the `tabulated-faith-pd-metadata.qzv` file with [QIIME 2 View](https://view.qiime2.org).
```