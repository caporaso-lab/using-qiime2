(metadata:validation)=
# How to validate metadata

QIIME 2 will automatically validate a metadata file anytime it is used.
This will inform you of any errors in your metadata formatting, which you can then correct.
To test this, you can use the `qiime metadata tabulate` command, which will read your tsv metadata file and produce a nicely formatting view in a QIIME 2 {term}`Visualization`