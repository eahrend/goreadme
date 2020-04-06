# goreadme

[![Build Status](https://travis-ci.org/posener/goreadme.svg?branch=master)](https://travis-ci.org/posener/goreadme)
[![codecov](https://codecov.io/gh/posener/goreadme/branch/master/graph/badge.svg)](https://codecov.io/gh/posener/goreadme)
[![GoDoc](https://godoc.org/github.com/posener/goreadme?status.svg)](http://godoc.org/github.com/posener/goreadme)
[![goreadme](https://goreadme.herokuapp.com/badge/posener/goreadme.svg)](https://goreadme.herokuapp.com)

Package goreadme creates readme markdown file from go doc.

#### Github Action

Github actions can be configured to update the README.md automatically every time it is needed.
Below there is an example that on every time a new change is pushed to the master branch, the
action is trigerred, generates a new README file, and if there is a change - commits and pushes
it to the master branch.

Add the following content to `.github/workflows/goreadme.md`:

```go
on:
  push:
    branches:
      - master
jobs:
    goreadme:
        runs-on: ubuntu-latest
        steps:
        - name: Check out repository
          uses: actions/checkout@v2
        - name: Update README.md according to Go doc
          uses: posener/goreadme@v1.2.1
          with:
            package_name: 'github.com/<your user>/<your project>'
            badge-travisci: 'true'
            badge-codecov: 'true'
            badge-godoc: 'true'
            badge-goreadme: 'true'
        - name: Commit and push changes
          run: |
            git add README.md
            if git diff --staged --exit-code; then exit 0; fi
            git config user.name Goreadme
            git config user.email posener@gmail.com
            git commit -m "Update readme according to Go doc"
            git push origin HEAD:$(echo "${GITHUB_REF}" | cut -d/ -f3)
```

Use as a command line tool

```go
$ go get github.com/posener/goreadme/...
$ goreadme -h
```

#### Why Should You Use It

Both go doc and readme files are important. Go doc to be used by your user's
library, and README file to welcome users to use your library. They share
common content, which is usually duplicated from the doc to the readme or vice versa
once the library is ready. The problem is that keeping documentation updated
is important, and hard enough - keeping both updated is twice as hard.

This library provides an easy way to create the one from the other. Using the
goreadme [Github App](https://github.com/apps/goreadme) makes it even easier.

#### Go Doc Instructions

The formatting of the README.md is done by the go doc parser. This makes the
Result README.md a bit more limited.
Currently, `goreadme` supports the formatting as explained
in [godoc page](https://blog.golang.org/godoc-documenting-go-code).
Meaning:

* A header is a single line that is separated from a paragraph above.

* Code block is recognized by indentation.

* Diff block is automatically detected:

```diff
-removed
 stay
+added
```

* Inline code is marked with backticks.

* URLs will just automatically be converted to links.

Additionally, some extra formatting was added.

* Local paths will be automatically converted to links, for example: [./goreadme.go](./goreadme.go).

* A URL and can have a title, as follows: [goreadme website](https://goreadme.herokuapp.com).

* A local path and can have a title, as follows: [goreadme main file](./goreamde.go).

* An image can be added: ![goreadme icon](./icon.png)

## Sub Packages

* [cmd/goreadme](./cmd/goreadme): Package main is a command line util that takes a Go repository and write to stdout the calculated README.md content.

---
Readme created from Go doc with [goreadme](https://github.com/posener/goreadme)
