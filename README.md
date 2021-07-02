<h1 align="center">kepubify</h1>

**Kepubify converts EPUBs to Kobo EPUBs.**

**[`Website`](https://pgaskin.net/kepubify/)** &nbsp; **[`Download`](https://pgaskin.net/kepubify/dl/)** &nbsp; **[`Web Version`](https://pgaskin.net/kepubify/try/)**

[![](https://img.shields.io/github/v/release/pgaskin/kepubify)](https://github.com/pgaskin/kepubify/releases/latest) [![](https://img.shields.io/drone/build/pgaskin/kepubify/master)](https://cloud.drone.io/pgaskin/kepubify) [![](https://img.shields.io/drone/build/pgaskin/kepubify/master?label=linux%20build)](https://cloud.drone.io/pgaskin/kepubify) [![](https://img.shields.io/appveyor/ci/pgaskin/kepubify/master?label=windows%20build)](https://ci.appveyor.com/project/pgaskin/kepubify/branch/master) [![](https://img.shields.io/travis/com/pgaskin/kepubify/master?label=macOS%20build)](https://travis-ci.com/pgaskin/kepubify) ![](https://img.shields.io/github/go-mod/go-version/pgaskin/kepubify) [![Go Reference](https://pkg.go.dev/badge/github.com/pgaskin/kepubify/v4.svg)](https://pkg.go.dev/github.com/pgaskin/kepubify/v4)

## About

Kepubify is standalone (it also works as a library or a webapp), converts most books in a fraction of a second (40-80x faster than Calibre), handles malformed HTML/XHTML gracefully without adding new issues to it, has multiple optional conversion options (punctuation smartening, custom CSS, text replacement, and more), has a full test suite, is interoperable with other applications, and is safe to use with untrusted books.

See the [releases](https://github.com/pgaskin/kepubify/releases/latest) page for
pre-built binaries for Windows, Linux, and macOS. See the [website](https://pgaskin.net/kepubify/) for more [documentation](https://pgaskin.net/kepubify/docs/), pre-built [binaries](https://pgaskin.net/kepubify/dl/) for Windows, Linux, and macOS, and a [web version](https://pgaskin.net/kepubify/try/).

## covergen/seriesmeta

Two other standalone utilities are included with kepubify. The
[`covergen`](./cmd/covergen) utility pre-generates cover images to speed up
library browsing on Kobo eReaders while providing higher-quality resizing. The
[`seriesmeta`](./cmd/seriesmeta) utility scans for EPUBs and KEPUBs, and updates
the Kobo database with the Calibre or EPUB3 series metadata. It works even if
the books haven't been imported yet. These utilities can be used independently
of Kepubify, and will still work properly without conflicts even if you use
Calibre or other ebook management software.

If you use `seriesmeta`, you may want to try my new
[`NickelSeries`](https://www.mobileread.com/forums/showthread.php?t=331680) mod
for Kobo eReaders instead. It seamlessly integrates with the Kobo firmware to
add built-in series metadata support for newly imported books, eliminating the
need to run `seriesmeta` manually every time new books are added.

## Building

Kepubify requires Go 1.16 or later. To install kepubify directly, run `go
install github.com/pgaskin/kepubify@latest`. To build from source, clone this
repository, and run `go build ./cmd/kepubify`.

On Go 1.17 or later, additional optimizations are automatically used to
significantly improve kepubify's performance by preventing unchanged files from
being re-compressed. To use a
[backported](https://github.com/pgaskin/kepubify/tree/forks/go116-zip.go117)
version of these optimizations on Go 1.16, add the option `-tags zip117` to the
build/install command. If you are using kepubify as a library in another
application with `-tags zip117` enabled on Go 1.16, it must also use the
backported package when passing a `*zip.Reader` to
`(*kepub.Converter).Transform`.

To build `seriesmeta`, a C compiler must be installed and CGO must be enabled.

Note that kepubify uses a custom
[fork](https://github.com/pgaskin/kepubify/tree/forks/html) of
[`golang.org/x/net/html`](https://pkg.go.dev/golang.org/x/net/html). This fork
provides additional options used by kepubify to allow reading malformed
HTML/XHTML and to produce polyglot HTML/XHTML output for maximum compatibility.
Previously, kepubify replaced it using a `replace` directive in `go.mod`, but
since the fork is now a standalone package, this is not necessary anymore, and
will no longer cause conflicts if used as a dependency in applications requiring
`golang.org/x/net/html` directly.

