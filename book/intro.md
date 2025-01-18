# *Using QIIME 2*

**Your guide to becoming a QIIME 2 Framework power user.**

```{admonition} Development status of this content
:class: note
*Using QIIME 2* is in an early stage of development, and as a result the documentation at https://docs.qiime2.org is still an important source of information for learning to use QIIME 2.
Over the next few months (as of 17 January 2025), existing content will be migrated and new content will developed.
Ultimately https://docs.qiime2.org will be retired.
While *Using QIIME 2* is in development, some URLs may change.

The canonical URL for this project is https://use.qiime2.org.
```

## Goals and development plan for *Using QIIME 2*

As the ecosystem of QIIME 2 {term}`distributions <distribution>` and {term}`plugins <plugin>` continues to grow, it has become necessary to make a split in the documentation between general-purpose information that describes how to use QIIME 2 itself **(i.e., "the QIIME 2 framework" 🌳)**, and how to apply the tools it supports to achieve your data analysis goals.
We know that you're interested in QIIME 2 primarily for the latter -- to achieve specific analysis goals related to microbiome data science -- so our goal is that *Using QIIME 2* serves primarily as a source for you to refer to when you need to accomplish specific tasks that are general to using the system.
That includes things like [using `Artifacts` as metadata](metadata-merge), replaying provenance[^replaying-provenance-issue], [creating and using an artifact cache](artifact-cache-tutorial) and [configuring your parallel computing environment](parallel-configuration).
It will also include explanations that can help you understand the system when you want to go deeper, including things like what QIIME 2 {term}`Artifacts <artifact>` (e.g., {term}`.qza <qza>` files) and {term}`Visualizations <visualization>` (e.g., {term}`.qzv <qzv>` files) are[^result-explanation-issue], and why you need to import your data into `Artifacts` before using QIIME 2[^importing-explanation-issue].
Generally speaking, **understanding these topics will help you understand what you're doing and empower you to become a QIIME 2 Framework power user, but they aren't strictly necessary to perform simple data analysis.**

In addition to *Using QIIME 2*, we are building distribution-specific and plugin-specific documentation.
**These will be designed to quickly get you started with performing your first data analysis with QIIME 2,** and will fall into a few categories.

### Distribution-specific documentation

The "old" user documentation at https://docs.qiime2.org serves this purpose for the *amplicon distribution*.
For example, you can start there to get instructions on how to [install QIIME 2](https://docs.qiime2.org/2024.10/install/), use it to carry out an [analysis of microbiome amplicon data](https://docs.qiime2.org/2024.10/tutorials/moving-pictures-usage/), and [see a reference](https://docs.qiime2.org/2024.10/plugins/) of the {term}`plugins <plugin>` and {term}`actions <action>` that are included in the *amplicon distribution*.
That user documentation predates the diverse distributions and plugins that now exist in the ecosystem - it therefore mixes distribution-specific information with the general purpose information that is currently being transitioned to *Using QIIME 2*.

### Plugin-specific documentation

Stand-alone plugins (i.e., those not included in existing distributions), like [q2-boots](https://github.com/caporaso-lab/q2-boots), will provide their own documentation linked from [the new QIIME 2 Library](https://library.qiime2.org)[^new-library-wip].
For now, you can refer to the [q2-boots documentation here](https://q2-boots.readthedocs.io/en/latest/), as an example of where we're going.

### Data-set specific documentation

Finally we'll also have some documentation that is focused on analysis of specific interesting data sets.
A good current example of this is the [QIIME 2 **C**ancer **A**utoFMT **T**utorial (q2-cat-book)](https://q2-cat-book.readthedocs.io/en/latest/).
This may sometimes cross distributions and stand-alone plugins, for example perhaps integrating tools from the *amplicon* and *metagenome* distributions to analyze paired 16S and metagenome data.

## Organization of *Using QIIME 2*

*Using QIIME 2* is organized under the [Diátaxis](https://diataxis.fr/) framework {cite}`diataxis`, which categorizes content into *sections* containing *Tutorials*, *How-To-Guides*, *Explanations*, and *References*.
Each serves a different goal for the reader:

```{list-table}
:header-rows: 1

* - Chapter
  - Purpose

* - Tutorials
  - Provide a guided exploration of a topic for **learning**.

* - How To Guides
  - Provide step-by-step instructions on how to **accomplish specific goals**.

* - Explanations
  - Provide a discussion intended to aid in **understanding** a specific topic.

* - References
  - Provide specific **information** (e.g., a list of utilities available through {term}`q2cli`).
```

You can navigate these sections on the left sidebar.

(acknowledgements)=
## Acknowledgements
*Using QIIME 2* is the result of past, present, and future (🤞) collaborative efforts.

The authors would like to thank [those who have contributed](https://github.com/qiime2/docs/graphs/contributors) to the writing of the original QIIME 2 User Documentation.
Some of the content in *Using QIIME 2* is sourced directly from that material.

The QIIME 2 Forum [moderators](https://forum.qiime2.org/g/q2-mods) and [community members](https://forum.qiime2.org/u?order=likes_received&period=all) have also been instrumental to the development of ideas and content presented here.

Finally, as this project gets further along, you can see [who has contributed directly to *Using QIIME 2*](https://github.com/caporaso-lab/using-qiime2/graphs/contributors).


## Getting Help
For the most up-to-date information on how to get help with QIIME 2, as a user or developer, see [here](https://github.com/qiime2/.github/blob/main/SUPPORT.md).

## Funding 🙏

This work was funded in part by NIH National Cancer Institute Informatics Technology for Cancer Research grant [1U24CA248454-01](https://reporter.nih.gov/project-details/9951750).

This book is built with MyST Markdown and Jupyter Book, which are supported in part with [funding](https://sloan.org/grant-detail/6620) from the Alfred P. Sloan Foundation.

Initial support for the development of QIIME 2 was provided through a [grant](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1565100) from the National Science Foundation.

## License

 <p xmlns:cc="http://creativecommons.org/ns#" xmlns:dct="http://purl.org/dc/terms/"><a property="dct:title" rel="cc:attributionURL" href="https://use.qiime2.org">Using QIIME 2</a> led by <a rel="cc:attributionURL dct:creator" property="cc:attributionName" href="https://cap-lab.bio">Greg Caporaso</a> is licensed under <a href="https://creativecommons.org/licenses/by-nc-nd/4.0/?ref=chooser-v1" target="_blank" rel="license noopener noreferrer" style="display:inline-block;">CC BY-NC-ND 4.0 <img style="height:22px!important;margin-left:3px;vertical-align:text-bottom;" src="https://mirrors.creativecommons.org/presskit/icons/cc.svg?ref=chooser-v1" alt=""><img style="height:22px!important;margin-left:3px;vertical-align:text-bottom;" src="https://mirrors.creativecommons.org/presskit/icons/by.svg?ref=chooser-v1" alt=""><img style="height:22px!important;margin-left:3px;vertical-align:text-bottom;" src="https://mirrors.creativecommons.org/presskit/icons/nc.svg?ref=chooser-v1" alt=""><img style="height:22px!important;margin-left:3px;vertical-align:text-bottom;" src="https://mirrors.creativecommons.org/presskit/icons/nd.svg?ref=chooser-v1" alt=""></a>.</p>

[^replaying-provenance-issue]: See [here](https://github.com/caporaso-lab/using-qiime2/issues/13).
[^new-library-wip]: The new QIIME 2 Library is in the early stages of development (as of 17 January 2025) - a lot of exciting new functionality for both users and developers is currently in progress!
For the time-being, you can still find the old QIIME 2 Library at https://old-library.qiime2.org.
[^result-explanation-issue]: See [here](https://github.com/caporaso-lab/using-qiime2/issues/11).
[^importing-explanation-issue]: See [here](https://github.com/caporaso-lab/using-qiime2/issues/12).