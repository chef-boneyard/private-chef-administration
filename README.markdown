# Private Chef Administration Guide

This repository has the source material for the Private Chef Administration guide. 

## Prerequisites

To use this documentation, you'll need to have [Sphinx](http://sphinx.pocoo.org) installed,
along with [Pygments](http://pygments.org) for syntax highlighting. If you want to generate
PDF documentation, you'll also need to have a version of [LaTeX](http://www.latex-project.org/)
installed (specifically latex2pdf).

## Installation

```bash
$ easy_install Pygments
$ easy_install sphinx
```

## Build

To build the documentation:

```bash
export LC_ALL=en_US.UTF
export LANG=en_US.UTF-8
$ make html
```

Will generate HTML output in build/html/index.html.

Run:

```bash
$ make help
```

For alternative formats.

## Publish

Use 'make upload' to upload to s3. Requires that you have Opscode internal credentials configured for s3cmd.
