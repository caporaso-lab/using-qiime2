# Using QIIME 2 source 📖

This repository contains the source for *Using QIIME 2*.
If you want to read *Using QIIME 2*, you can find it at https://use.qiime2.org.

## Development instructions

The following sub-sections illustrate how to develop this documentation.

### Create the development environment

To build this documentation locally for development purposes, first create your development environment.

```
cd using-qiime2
__Q2DEV_ENV_NAME=uq2-$(date "+%Y-%m-%d")
conda env create -n $__Q2DEV_ENV_NAME$ --file environment-files/readthedocs.yml
conda activate $__Q2DEV_ENV_NAME
```

### Build the book

Next, build the book:

```
make html
```

### Serve the book locally

Finally, run the following to serve the built documentation locally:

```
make serve
```

Have fun!
