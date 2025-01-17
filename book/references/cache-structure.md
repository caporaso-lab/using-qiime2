(cache-structure)=
# Structure of an Artifact Cache

The format used to represent an Artifact Cache on disk is versioned, and changes may be made over time.
Each of the sections below presents information on a specific version of the Artifact Cache format.
As of this writing (16 January 2024), there is only one version.

## Artifact Cache format version 1

Assuming you have a version 1 artifact cache named `my-artifact-cache`, you can expect to find the structure of the files within it to look like the following:

```
my-artifact-cache/
├── data/
│   ├── uuid1/
│   ├── uuid2/
│   ├── uuid3/
│   └── uuid4/
├── keys/
│   ├── bar.yaml
│   ├── baz.yaml
│   └── foo.yaml
├── pools/
│   └── puuid1/
│       ├── uuid1 -> ../../data/uuid1/
│       └── uuid2 -> ../../data/uuid2/
├── processes/
│   └── <process-id>-<process-create-time>@<user>/
│       ├── uuid3 -> ../../data/uuid3/
│       └── uuid4 -> ../../data/uuid4/
└── VERSION
```

The top-level entries in this directory are as follows:
- `data`: The data directory contains all of the artifacts in the cache in unzipped form.
- `keys`: The keys directory contains yaml files that refer to either a piece of data or a pool. The data/pool referenced by the key will be kept as long as the key exists.
- `pool`: The pools directory contains all named (keyed) pools in the cache.
Each pool contains symlinks to all of the data it contains.
- `processes`: The processes directory contains process pools of the format `<process-id>-<process-create-time>@<user>` for each process that has used this cache.
Each pool contains symlinks to each element in the data directory the process that created the pool has used in some way (created, loaded, etc.).
These symlinks are ephemeral and have lifetimes <= the lifetime of the process that created them.
More permanent storage is done using keys.
- `VERSION`: This file contains some information QIIME 2 uses to determine what version of QIIME 2 was used to create the cache and what version of cache it is (if we make breaking changes in the future this version number will allow for backwards compatibility).