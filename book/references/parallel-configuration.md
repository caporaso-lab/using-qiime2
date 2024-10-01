(parallel-configuration)=
# Parallel Pipeline configuration

QIIME 2 provides formal support for parallel computing of {term}`Pipelines <pipeline>` through [Parsl](https://parsl.readthedocs.io/en/stable/1-parsl-introduction.html>).

## Parsl configuration

A [Parsl configuration](https://parsl.readthedocs.io/en/stable/userguide/configuring.html) tells Parsl what resources are available and how to use them, and is required to use Parsl.
The [Parsl documentation](https://parsl.readthedocs.io/en/stable/) provides full detail on [Parsl configuration](https://parsl.readthedocs.io/en/stable/userguide/configuring.html#).

In the context of QIIME 2, Parsl configuration information is maintained in a QIIME 2 configuration file.
QIIME 2 configuration files are stored on disk in [TOML](https://toml.io/en/) files.

### Default Parsl configuration

For basic multi-processor usage, QIIME 2 writes a default configuration file the first time it's needed (e.g., if you instruct QIIME 2 to execute in parallel without a particular configuration).

The default `qiime2_config.toml` file, as of QIIME 2 2024.10, looks like the following:

(default-parsl-configuration-file)=
```
[parsl]
strategy = "None"

[[parsl.executors]]
class = "ThreadPoolExecutor"
label = "tpool"
max_threads = ...

[[parsl.executors]]
class = "HighThroughputExecutor"
label = "default"
max_workers = ...

[parsl.executors.provider]
class = "LocalProvider"
```

When this file is written to disk, the `max_threads` and `max_workers` values (represented above by `...`) are computed by QIIME 2 as one less than the CPU count on the computer where it is running (`max(psutil.cpu_count() - 1, 1)`).

This configuration defines two `Executors`.

1. The [`ThreadPoolExecutor`](https://parsl.readthedocs.io/en/stable/stubs/parsl.executors.ThreadPoolExecutor.html?highlight=Threadpoolexecutor) that parallelizes jobs across multiple threads in a process.
2. The [`HighThroughputExecutor`](https://parsl.readthedocs.io/en/stable/stubs/parsl.executors.HighThroughputExecutor.html?highlight=HighThroughputExecutor) that parallelizes jobs across multiple processes.

In this case, the `HighThroughputExecutor` is designated as the default by nature of it's `label` value being `default`.
Your parsl configuration **must** define an executor with the label `default`, and this is the executor that QIIME 2 will use to dispatch your jobs to if you do not specify an alternative.

````{admonition} The parsl.Config object
:class: tip

This parsl configuration is ultimately read into a `parsl.Config` object internally in QIIME 2.
The `parsl.Config` object that corresponds to the above example would look like the following:

```python
config = parsl.Config(
    executors=[
        ThreadPoolExecutor(
            label='tpool',
            max_threads=... # will be an integer value
        ),
        HighThroughputExecutor(
            label='default',
            max_workers=..., # will be an integer value
            provider=LocalProvider()
        )
    ],
    strategy=None
)
```
````

### Parsl configuration, line-by-line

This first line of [the default configuration file presented above](default-parsl-configuration-file) indicates that this is the parsl section (or [table](https://toml.io/en/v1.0.0#table), to use TOML's terminology) of our configuration file.

```
[parsl]
```

The next line:

```
strategy = "None"
```

is a top-level Parsl configuration parameter that you can [read more about in the Parsl documentation](https://parsl.readthedocs.io/en/stable/userguide/configuring.html#multi-threaded-applications).
This may need to be set differently depending on your system.

If you were to load this into Python using tomlkit you would get the following dictionary:

Next, the first executor is added.

```
[[parsl.executors]]
class = "ThreadPoolExecutor"
label = "tpool"
max_threads = 7
```

The double square brackets (`[[ ... ]]`) indicates that [this is an array](https://toml.io/en/v1.0.0#array-of-tables), `executors`, that is nested under the `parsl` table.
`class` indicates the specific parsl class that is being configured ([`parsl.executors.ThreadPoolExecutor`](https://parsl.readthedocs.io/en/stable/stubs/parsl.executors.ThreadPoolExecutor.html#parsl.executors.ThreadPoolExecutor) in this case); `label` provides a label that you can use to refer to this executor elsewhere; and `max_threads` is a configuration value for the ThreadPoolExecutor class which corresponds to a parameter name for the class's constructor.
In this example a value of 7 is specified for `max_threads`, but as noted above this will be computed specifically for your machine when this file is created.

Parsl's `ThreadPoolExecutor` runs on a single node, so we provide a second executor which can utilize up to 2000 nodes.

```
[[parsl.executors]]
class = "HighThroughputExecutor"
label = "default"
max_workers = 7

[parsl.executors.provider]
class = "LocalProvider"
```

The definition of this executor, [`parsl.executors.HighThroughputExecutor`](https://parsl.readthedocs.io/en/stable/stubs/parsl.executors.HighThroughputExecutor.html#parsl.executors.HighThroughputExecutor), looks similar to the definition of the `ThreadPoolExecutor`, but it additionally defines a `provider`.
The provider class provides access to computational resources.
In this case, we use [`parsl.providers.LocalProvider`](https://parsl.readthedocs.io/en/stable/stubs/parsl.providers.LocalProvider.html), which provides access to local resources (i.e., on the laptop or workstation).
[Other providers are available as well](https://parsl.readthedocs.io/en/stable/reference.html#providers), including for Slurm, Amazon Web Services, Kubernetes, and more.

### Mapping {term}`Actions <action>` to executors

An executor mapping can be added to your parsl configuration that defines which actions should run on which executors.
If an action is unmapped, it will run on the default executor.
This can be specified as follows:

```
[parsl.executor_mapping]
action_name = "tpool"
```

```{warning}
The mechanism for specifying action names at present does not handle the case of different plugins defining actions with the same name.
This mechanism will likely change soon, and may be a {term}`breaking change`.
You can track progress on this [here](https://github.com/qiime2/qiime2/issues/802).
```

(view-parsl-configuration)=
### Viewing the current configuration

Using {term}`q2cli`, you can see your current `qiime2_config.toml` file by running:

```shell
qiime info --config-level 2
```

(qiime2-configuration-precedence)=
### QIIME 2 configuration file precedence

When QIIME 2 needs configuration information, the following precedence order is followed to load a configuration file:

1. The path specified in the environment variable `$QIIME2_CONFIG`.
2. The file at `<user_config_dir>/qiime2/qiime2_config.toml`
3. The file at `<site_config_dir>/qiime2/qiime2_config.toml`
4. The file at `$CONDA_PREFIX/etc/qiime2_config.toml`

If no configuration is found after checking those four locations, QIIME 2 writes a default configuration file to `$CONDA_PREFIX/etc/qiime2_config.toml` and uses that.
This implies that after your first time running QIIME 2 in parallel without a config in at least one of the first 3 locations, the path referenced in step 4 will exist and contain a configuration file.

Alternatively, when using {term}`q2cli`, you can provide a specific configuration for use in configuring parsl using the `--parallel-config` option.
If provided, this overrides the priority order above.

Similarly, when using the {term}`Python 3 API`, you can provide a specific configuration by passing a `parsl.Config` object into your `ParallelConfig` context manager.

````{admonition} user_config_dir and site_config_dir
:class: note
On Linux, `user_config_dir` will usually be `$HOME/.config/qiime2/`.
On macOS, it will usually be `$HOME/Library/Application Support/qiime2/`.

You can get find the directory used on your system by running the following command:

```bash
python -c "import appdirs; print(appdirs.user_config_dir('qiime2'))"
```

On Linux `site_config_dir` will usually be something like `/etc/xdg/qiime2/`, but it may vary based on Linux distribution.
On macOS it will usually be `/Library/Application Support/qiime2/`.

You can get find the directory used on your system by running the following command:

```bash
python -c "import appdirs; print(appdirs.site_config_dir('qiime2'))"
```
````
