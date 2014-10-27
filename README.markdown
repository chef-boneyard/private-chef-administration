# Private Chef Administration Guide

ATTN: Chef 12 is the  new Chef server! Please see the documentation at http://docs.getchef.com/server/. 

ATTN: The documentation for Private Chef has been moved to https://github.com/opscode/chef-docs and is published to http://docs.opscode.com/release/private_chef/index.html. This content is no longer actively maintained.


This repository has the source material for the Private Chef Administration guide.

## Prerequisites

To use this documentation, you'll need to have [Sphinx](http://sphinx.pocoo.org) installed,
along with [Pygments](http://pygments.org) for syntax highlighting. If you want to generate
PDF documentation, you'll also need to have a version of [LaTeX](http://www.latex-project.org/)
installed (specifically latex2pdf).

## Installation

```bash
    easy_install Pygments
    easy_install sphinx
```

## Build

To build the documentation:

```bash
    export LC_ALL=en_US.UTF
    export LANG=en_US.UTF-8
    make html
```

Will generate HTML output in build/html/index.html.

Run:

```bash
    make help
```

For alternative formats.

## Publish

Use 'make upload' to upload to s3. Requires that you have Opscode internal credentials configured for s3cmd.

## OSX Instructions

### Using Homebrew Python and avoiding permission issues

```bash
    brew install python
```

Now add the following to your PATH using the shell initialization
script that makes sense for your environment:

```bash
   export PATH=/usr/local/share/pythno:$PATH
```

You can now call `easy_install` directly with no sudo and then follow
the make instructions.

### Using Python shipped with OSX

A few minor nits for installing on OSX.

```bash
    sudo easy_install Pygments
    sudo easy_install sphinx
```

You may have issues with ownership of `/Library/Python/2.7/site-packages/` after you build Pygments and sphinx. You can either change the ownership with:

```bash
    sudo chown YOURUSER -R /Library/Python/2.7/site-packages
```

or make them world-executable with

```bash
    sudo chmod -R 755 /Library/Python/2.7/site-packages
```

You can then use the `make` commands as previously documented.

iPad
----

If you want iPad-readable docs, take the output of

```bash
    make epub
```

and copy the `build/epub/PrivateChefGuide.epub` to iTunes and sync it into the iBooks app.
