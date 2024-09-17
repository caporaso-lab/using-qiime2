# How to merge metadata

Since metadata can come from many different sources, QIIME 2 supports metadata merging when running commands.
Building upon the examples above, simply passing ``--m-input-file`` multiple times will combine the metadata columns in the specified files:

```shell
qiime metadata tabulate \
    --m-input-file sample-metadata.tsv \
    --m-input-file faith_pd_vector.qza \
    --o-visualization tabulated-combined-metadata.qzv
```

The resulting metadata after the merge will contain the intersection of the identifiers across all of the specified files.
In other words, the merged metadata will only contain identifiers that are shared across all provided metadata files.
This is an *inner join* using database terminology.

```{admonition} Exercise
Modify the command above to merge the [evenness vector](https://docs.qiime2.org/2021.4/data/tutorials/moving-pictures/core-metrics-results/evenness_vector.qza) of ``SampleData[AlphaDiversity]`` after the Faith's PD vector.
What happens when merging the three artifacts?
How many columns are present in the resulting metadata visualization?
How many of those columns represent the sample IDs?
How many of those columns represent ``SampleData[AlphaDiversity]`` metrics?
What happens to the visualization if the order of the metadata files is reversed?
Hint, take a closer look at the column ordering.
```

Metadata merging is supported anywhere that metadata is accepted in QIIME 2.
For example, it might be interesting to color an Emperor plot based on the study metadata, or sample alpha diversity.
This can be accomplished by providing both the sample metadata file *and* the ``SampleData[AlphaDiversity]`` artifact:

```shell
curl -sL \
  "https://data.qiime2.org/2021.4/tutorials/metadata/unweighted_unifrac_pcoa_results.qza" > \
  "unweighted_unifrac_pcoa_results.qza"

qiime emperor plot \
    --i-pcoa unweighted_unifrac_pcoa_results.qza \
    --m-metadata-file sample-metadata.tsv \
    --m-metadata-file faith_pd_vector.qza \
    --o-visualization unweighted-unifrac-emperor-with-alpha.qzv
```

```{admonition} Exercise
What body sites are associated with the highest Faith's phylogenetic diversity value?
Hint: first color by body site, and then color by Faith's PD using a continuous color scheme.
```