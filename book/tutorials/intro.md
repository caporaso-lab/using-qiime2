(tutorial-setup)=
# Getting started

The tutorials in *Using QIIME 2* provide basic information on how to use the QIIME 2 framework, {term}`q2cli` (i.e., the official QIIME 2 command line interface), and the QIIME 2 Python 3 API.
The tutorials make use of the *Tiny Distribution*, and the QIIME 2 example plugin `q2-dwq2`.
This deployment allows for in-depth learning of how QIIME 2 itself works, and the information you learn here will be relevant across all QIIME 2 distributions.
When you're ready to perform your own data analysis you'll transition to domain-specific plugins or distributions and their documentation.

Before attempting to run the *Using QIIME 2* tutorials, configure your learning environment following the steps here.

## Install the QIIME 2 "Tiny Distribution"

The QIIME 2 "Tiny Distribution" is a minimal set of QIIME 2 functionality for building and using plugins through the QIIME 2 command line or Python 3 API.
Install the most recent release version of the *QIIME 2 Tiny Distribution* using the [instructions on the QIIME 2 Library](https://library.qiime2.org/quickstart/tiny).

## Activate the ``conda`` environment and test your installation

You can now activate the environment you just created using `conda activate`, as described in the installation instructions.

To test your QIIME 2 environment, run:

```bash
qiime info
```

You should see something like the following, though the version numbers you'll see may be different:

```
System versions
Python version: 3.10.14
QIIME 2 release: 2025.4
QIIME 2 version: 2025.4.0
q2cli version: 2025.4.0

Installed plugins
metadata: 2025.4.0
types: 2025.4.0

...
```

At this stage you have a working QIIME 2 environment, but it doesn't do a whole lot.
To add some bioinformatics functionality, we'll next add the QIIME 2 example plugin [`q2-dwq2`](https://github.com/caporaso-lab/q2-dwq2).

## Install q2-dwq2

All domain-specific functionality in QIIME 2 comes in the form of plugins.
Sometimes you'll install these directly, and sometimes you'll install QIIME 2 distributions which are bundles of plugins intended to be used together.
In this case, we're going to install one specific plugin.
Run the following command from your `using-qiime2` conda environment (i.e., after having run `conda activate using-qiime2`).

```shell
pip install https://github.com/caporaso-lab/q2-dwq2/archive/refs/heads/main.zip
```

If you run `qiime info` again, you should now see a new plugin, `dwq2`, in the list of *Installed Plugins*.

```
System versions
Python version: 3.10.14
QIIME 2 release: 2025.4
QIIME 2 version: 2025.4.0
q2cli version: 2025.4.0

Installed plugins
dwq2: 0+unknown
metadata: 2025.4.0
types: 2025.4.0

...
```

## Exploring the available functionality

To see the list of available plugins, along with some additional information, run:

```shell
qiime --help
```

To see what functionality, or {term}`Actions <action>` a plugin defines, call help on that plugin:

```shell
qiime dwq2 --help
```

To learn how to use a specific action, call help on it:

```shell
qiime dwq2 nw-align --help
```

Take a few minutes now to explore `q2-dwq2`.
What is this plugin intended to do?
What is some of the functionality that it provides?
