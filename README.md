# ORIGIN Frame Implementation in Go

This fork of the Go programming language includes support for HTTP/2 ORIGIN Frames. The changes include the introduction
of the `OriginPusher` interface in addition to the changes to the `net` module which is also maintained as a fork 
[here](https://github.com/cloudflare/net-originframe).

### Build instructions:

The instruction below assume that the working directory is `code`

```shell
$ git clone https://github.com/cloudflare/go-originframe go
# Clone the modified net module with the changes to support HTTP/2 ORIGIN Frames
$ git clone https://github.com/cloudflare/net-originframe net
$ cd go/src
$ ./make.bash # build go, This will generate `bin/go` and `bin/gofmt`
```

To make modifications to `net` and bundle the changes into `h2_bundle.go`, export the `go` directory into `GOROOT`
and set a corresponding `GOPATH`. Obtain the latest `bundle` tool by running

```shell
$ go install golang.org/x/tools/cmd/bundle@latest
```

Generate the `h2_bundle.go` by running:

```shell
$ cd net
$ go generate -run=bundle std
```

The bundled `h2_bundle.go` is now available in `go/src/net/http` with the implementation of the ORIGIN Frames. 

Please refer to [this file](./example.md) for an example implementation.

# The Go Programming Language

Go is an open source programming language that makes it easy to build simple,
reliable, and efficient software.

![Gopher image](https://golang.org/doc/gopher/fiveyears.jpg)
*Gopher image by [Renee French][rf], licensed under [Creative Commons 3.0 Attributions license][cc3-by].*

Our canonical Git repository is located at https://go.googlesource.com/go.
There is a mirror of the repository at https://github.com/golang/go.

Unless otherwise noted, the Go source files are distributed under the
BSD-style license found in the LICENSE file.

### Download and Install

#### Binary Distributions

Official binary distributions are available at https://go.dev/dl/.

After downloading a binary release, visit https://go.dev/doc/install
for installation instructions.

#### Install From Source

If a binary distribution is not available for your combination of
operating system and architecture, visit
https://go.dev/doc/install/source
for source installation instructions.

### Contributing

Go is the work of thousands of contributors. We appreciate your help!

To contribute, please read the contribution guidelines at https://go.dev/doc/contribute.

Note that the Go project uses the issue tracker for bug reports and
proposals only. See https://go.dev/wiki/Questions for a list of
places to ask questions about the Go language.

[rf]: https://reneefrench.blogspot.com/
[cc3-by]: https://creativecommons.org/licenses/by/3.0/
