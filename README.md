# codoc

**codoc** is a new OCaml documentation tool **which allows to/which adds …**. It does not impose any
  specific package manager on you or assume a specific
  workflow. **codoc** requires at least **OCaml** `4.02.2`.

## Quick Start

You first need to set-up an opam switch with the right environment. We
are working on making our tools and patches properly integrated
upstream, so in the future these steps won't be necessary.

```sh
export OPAMKEEPBUILDDIR=1
unset OCAMLPARAM
opam switch doc -A 4.02.2 # Switch into an empty `4.02.2` switch
export OCAMLPARAM=_,bin-annot=1
eval `opam config env`
```

Once this is done, you can install your opam packages as usual.

```sh
opam install ... # Install your package set.
```

Finally, installing **codoc** itself will be useful.

```sh
opam install codoc
```

**codoc** supplies an **opam doc** command to generate cross-referenced
  documentation for all packages on an opam switch.  This program uses
  **codoc**'s scriptable CLI and and also offers an easy-to-use
  `--serve` option for immediately starting a web server for your opam
  installation's documentation. Using **opam doc** is as easy as:

```sh
opam doc my-opam-doc/
```

The HTML documentation for all packages on the current switch will be
created in `my-opam-doc/` by **opam doc**. When using `--serve`, the
documentation will be generated for the HTTP scheme (omitting
`index.html`) and `cohttp-server-lwt` will be started in the output
directory.

## The `codoc` Tool

To generate HTML documentation from a build directory containing files
compiled with **OCaml** `4.02.2`, simply run:

```sh
codoc doc [CMTI_DIR] -o [OUTPUT_DIR]
```

**codoc** will recursively search for `.cmti` files in *CMTI_DIR* and
  use their relative paths to construct a parallel directory hierarchy
  in *OUTPUT_DIR*. Of course, you can also run **codoc** on individual
  files and *OUTPUT_DIR* (or *OUTPUT_FILE*) does not need to exist.

**codoc** will refuse to overwrite already extracted or rendered
  documentation unless the `-f` flag is provided.

**codoc** understands package hierarchies. To output documentation for a
  specific package, simply pass `--package` followed by a
  `slash/separated/package/path`. The *OUTPUT_DIR* will be the base of
  the hierarchy and the documentation inside will cross-reference.

`codoc doc` has some other options that are described below in the
  section about the `html` subcommand.

## Scriptable Command Line

The `doc` subcommand of **codoc** is actually a composition of 3
different subcommands: `extract`, `link`, and `html`. Each of these
commands can take the `-f` and `--index` arguments. Documentation
indexes can be generated by providing the `--index` flag.

The `html` subcommand takes extra options, `--css`, `--scheme`,
and `--share`. It does not yet support `--package`, though, [support
is planned](https://github.com/dsheets/codoc/issues/42). Of these,
`--scheme file` is the most useful for browsing documentation in a web
browser directly off of the disk.

## Interfaces

The toolchain that **codoc** realizes was designed to have many
interfaces to subcomponents should you wish to customize your
documentation generation more radically than the tool provides.

[octavius](https://github.com/lpw25/octavius) parses documentation
comments written in `ocamldoc` format.

[doc-ock](https://github.com/lpw25/doc-ock) parses OCaml interfaces,
exposes types for signatures and identifiers, and defines resolution
procedures. **doc-ock** uses **octavius** for documentation comment
parsing.

[doc-ock-xml](https://github.com/lpw25/doc-ock-xml/) defines XML
serialization and parsing for the types in **doc-ock**. This XML
format will eventually provide mechanism for extensible documentation
content.

**codoc** defines a documentation index XML format for tracking package
  hierarchies, documentation issues, and hierarchically localized
  configuration. **codoc** also defines a scriptable command-line
  interface.

Most of these interfaces are not stable. The basics of **codoc**'s CLI,
though, are likely to remain consistent.

## Contributions

Contributions are very welcome! Please feel free to report issues,
ideas, comments, or concerns to the [codoc issue
tracker](https://github.com/dsheets/codoc/issues).

## Documentation Feature Tests

See
[dsheets/ocamlary-test-library](https://github.com/dsheets/ocamlary-test-library)
for some documentation feature tests.
