[![npm version](https://img.shields.io/npm/v/print-nonascii.svg)](https://npmjs.com/package/print-nonascii)[![license](https://img.shields.io/badge/license-MIT-blue.svg)](https://github.com/mklement0/print-nonascii/blob/master/LICENSE.md)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

**Contents**

- [print-nonascii: print lines that contain non-ASCII characters.](#print-nonascii-print-lines-that-contain-non-ascii-characters)
- [Examples](#examples)
- [Installation](#installation)
  - [Prerequisites](#prerequisites)
  - [Installation from the npm registry](#installation-from-the-npm-registry)
  - [Manual installation](#manual-installation)
- [Usage](#usage)
- [License](#license)
  - [Acknowledgements](#acknowledgements)
  - [npm dependencies](#npm-dependencies)
- [Changelog](#changelog)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# print-nonascii: print lines that contain non-ASCII characters.

`print-nonascii` is a Unix CLI that locates lines in text files or   
stdin input that contain non-ASCII characters, which is helpful when  
diagnosing character encoding problems.

Lines can be printed as-is and/or using abstract representations of non-ASCII
characters in one of several formats; namely:

* `-v`, `--caret` ... the same representation `cat -v` uses, based on caret notation.
* `--bash` ... per-_byte_ two-digit hex. escape sequences such as `\xc3`
* `--psh` ... PowerShell Unicode escape sequences such as `` `u{20ac} `` for `€`

Note: `--psh` only works correctly with properly UTF-8-encoded input.

Line numbers can be prepended on request, and output for multiple input
files is by default preceded with headers identifying each input file.

_Caveat: For now, no automated tests are run before releases._

# Examples

```sh
# Create a test file with 1 line containing a non-ASCII character.
$ cat <<'EOF' > /tmp/test.txt
one
twö
three
EOF

# Print only lines that have non-ASCII characters, as-is.
$ print-nonascii /tmp/test.txt
twö

# Print only lines that have non-ASCII characters, with line numbers:
$ print-nonascii -n /tmp/test.txt
2:twö

# Print only lines that have non-ASCII characters, using PowerShell 
# Unicode escape-sequence notation (--psh), preceded by the 
# line as-is (--raw).
# The Unicode code point of character "ö" is U+00F6:
$ print-nonascii --psh --raw /tmp/test.txt
twö
tw`u{f6}

# Ditto with line numbers and per-byte Bash escape sequences:
$ print-nonascii --bash --raw /tmp/test.txt
twö
tw\xc3\xb6

# Simulate input from multiple files by specifying the same file
# twice, so as to show the headers identifying each input file 
# (suppress with -b).
# Note that each header line (invisibly) starts with control 
# character U+0001, so as to allow more predictable
# identification of header lines in the output.
$ print-nonascii -n /tmp/test.txt /tmp/test.txt 
###	/tmp/test.txt
2:twö
###	/tmp/test.txt
2:twö
```

# Installation

## Prerequisites

* When installing from the **npm registry**: **macOS** and **Linux**
* When installing **manually**: any Unix platform with `bash` that also has `perl` installed.

## Installation from the npm registry

With [Node.js](http://nodejs.org/) installed, install [the package](https://www.npmjs.com/package/print-nonascii) as follows:

    [sudo] npm install print-nonascii -g

**Note**:

<sup>Note: Even if you don't use Node.js, its package manager, `npm`, works across platforms and is easy to install; try [`curl -L https://git.io/n-install | bash`](https://github.com/mklement0/n-install)</sup>

* Whether you need `sudo` depends on how you installed Node.js / io.js and whether you've [changed permissions later](https://docs.npmjs.com/getting-started/fixing-npm-permissions); if you get an `EACCES` error, try again with `sudo`.
* The `-g` ensures [_global_ installation](https://docs.npmjs.com/getting-started/installing-npm-packages-globally) and is needed to put `print-nonascii` in your system's `$PATH`.

## Manual installation

* Download [the CLI](https://raw.githubusercontent.com/mklement0/print-nonascii/stable/bin/print-nonascii) as `print-nonascii`.
* Make it executable with `chmod +x print-nonascii`.
* Move it or symlink it to a folder in your `$PATH`, such as `/usr/local/bin` (macOS) or `/usr/bin` (Linux).

# Usage

Find concise usage information below; for complete documentation, read the [manual online](doc/print-nonascii.md), or, once installed, run `man print-nonascii` (`print-nonascii --man` if installed manually).

<!-- DO NOT EDIT THE FENCED CODE BLOCK and RETAIN THIS COMMENT: The fenced code block below is updated by `make update-readme/release` with CLI usage information. -->

```nohighlight
$ print-nonascii --help


Prints lines that contain non-ASCII characters.

    print-nonascii [--<mode> [-r]] [-n] [-b] [file ...]
    print-nonascii -q                        [file ...]

    --<mode> prints abstract representations of non-ASCII chars.; one of:
      --caret, -v ... use caret notation, as cat -v would.
      --bash ... represent non-ASCII bytes as \xhh 
      --psh ... (PowerShell) represent non-ASCII Unicode characters as  
                Unicode escape sequences: <backtick>u{h...}
    
    -r, --raw ... with --<mode>, print each matching line as-is too, first.

    -n, --line-number ... prefix the output lines with their line number from  
     the original file, using format "<line-number>:" - decimal line numbers,  
     no padding, no space before or after the ":"

    -b, --bare ... suppress per-input-filename headers

    -q ... quiet mode: produce no output; signal presence of non-ASCII chars.  
           with exit code 0; exit code 100 signals that there are none.

Standard options: --help, --man, --version, --home
```

<!-- DO NOT EDIT THE NEXT CHAPTER and RETAIN THIS COMMENT: The next chapter is updated by `make update-readme/release` with the contents of 'LICENSE.md'. ALSO, LEAVE AT LEAST 1 BLANK LINE AFTER THIS COMMENT. -->

# License

Copyright (c) 2017 Michael Klement <mklement0@gmail.com> (http://same2u.net), released under the [MIT license](https://spdx.org/licenses/MIT#licenseText).

## Acknowledgements

This project gratefully depends on the following open-source components, according to the terms of their respective licenses.

[npm](https://www.npmjs.com/) dependencies below have an optional suffix denoting the type of dependency: the *absence* of a suffix denotes a required *run-time* dependency; `(D)` denotes a *development-time-only* dependency, `(O)` an *optional* dependency, and `(P)` a *peer* dependency.

<!-- DO NOT EDIT THE NEXT CHAPTER and RETAIN THIS COMMENT: The next chapter is updated by `make update-readme/release` with the dependencies from 'package.json'. ALSO, LEAVE AT LEAST 1 BLANK LINE AFTER THIS COMMENT. -->

## npm dependencies

* [doctoc (D)](https://github.com/thlorenz/doctoc#readme)
* [json (D)](https://github.com/trentm/json#readme)
* [marked-man (D)](https://github.com/kapouer/marked-man#readme)
* [replace (D)](https://github.com/harthur/replace#readme)
* [semver (D)](https://github.com/npm/node-semver#readme)
* [urchin (D)](https://git.sdf.org/tlevine/urchin)

<!-- DO NOT EDIT THE NEXT CHAPTER and RETAIN THIS COMMENT: The next chapter is updated by `make update-readme/release` with the contents of 'CHANGELOG.md'. ALSO, LEAVE AT LEAST 1 BLANK LINE AFTER THIS COMMENT. -->

# Changelog

Versioning complies with [semantic versioning (semver)](http://semver.org/).

<!-- RETAIN THIS COMMENT. An entry template for a new version is automatically added each time `make version` is called. Fill in changes afterward. -->

* **[v0.0.3](https://github.com/mklement0/print-nonascii/compare/v0.0.2...v0.0.3)** (2017-09-11):
  * [enhancement] Header lines are now only printed for input files that produce at least 1 output line.

* **[v0.0.2](https://github.com/mklement0/print-nonascii/compare/v0.0.1...v0.0.2)** (2017-09-10):
  * [fix] Header line is no longer printed twice when `--<mode>` is combined with `--raw`.
  * Header line now uses a tab char. to separate prefix `###` from the filename.

* **v0.0.1** (2017-09-10):
  * Initial release.
