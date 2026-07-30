# clojerl

[![Build](https://github.com/antlobach/clojerl/actions/workflows/build.yml/badge.svg)](https://github.com/antlobach/clojerl/actions/workflows/build.yml)
[![Fork release](https://img.shields.io/github/v/release/antlobach/clojerl)](https://github.com/antlobach/clojerl/releases)
[![Upstream Hex package](https://img.shields.io/hexpm/v/clojerl.svg)](https://hex.pm/packages/clojerl)

Clojure implemented on the Erlang VM. This community-maintained fork keeps
Clojerl working on current Erlang/OTP releases.

Fork releases are published on [GitHub](https://github.com/antlobach/clojerl/releases)
and as `ghcr.io/antlobach/clojerl` container images. The Hex badge reports the
original project's `clojerl` package.

## Origins and maintenance

[Juan Facorro][original-author] created Clojerl, with substantial work from the
[original Clojerl contributors][contributors]. This fork preserves their
history, license, and authorship while providing ongoing compatibility
maintenance. The original project remains available at
[`clojerl/clojerl`][upstream].

## Building

This fork is tested on **Erlang/OTP 24 through 28** and requires
[*rebar3*][rebar3].

```shell
git clone https://github.com/antlobach/clojerl
cd clojerl
make
```

On Windows:

```shell
git clone https://github.com/antlobach/clojerl
cd clojerl
rebar3 clojerl compile
```

## Getting Started

### Documentation and examples

Start with this README and the examples in [`scripts/examples`][examples].
The [original Clojerl documentation][upstream-docs] remains useful for project
history and language design, but may not describe current fork behavior.

### Container REPL

Release images are published from this fork to GitHub Container Registry:

```shell
docker pull ghcr.io/antlobach/clojerl:0.9.1
docker run --rm -it ghcr.io/antlobach/clojerl:0.9.1
```

To build the image locally:

```shell
docker build -f scripts/Dockerfile -t antlobach/clojerl:0.9.1 .
docker run --rm -it antlobach/clojerl:0.9.1
```

The REPL starts with:

```clojure
Clojure 0.9.1
clje.user=>
```

### Local REPL

Run `make repl`. On Windows, first run `rebar3 clojerl compile`, then
`bin/clje.bat`.

```text
Clojure 0.9.1
clje.user=>
```

From the REPL, evaluate Clojerl expressions:

```clojure
clje.user=> (map inc (range 10))
(1 2 3 4 5 6 7 8 9 10)
```

### Code examples

The [`scripts/examples`][examples] directory demonstrates Clojerl special forms
and Erlang VM interop.

### Web application example

The original project maintains a basic
[Cowboy web application example][example-web-app].

### Building your own application

[`rebar3_clojerl`][rebar3_clojerl] integrates Clojerl with Erlang's `rebar3`
build tool. It can create a project, compile code, run tests, and start a REPL.
Refer to the plugin repository for its command documentation.

## Rationale

Erlang is a great language for building safe, reliable and scalable
systems. It provides immutable, persistent data structures
out of the box and its concurrency semantics are unequalled by any
other language.

Clojure is a Lisp and as such comes with all the goodies Lisps
provide. Apart from these Clojure also introduces powerful
abstractions such as protocols, multimethods and seqs, to name a few.

Clojure was built to simplify the development of concurrent programs
and some of its concurrency abstractions could be adapted to Erlang.
It is fair to say that combining the power of the Erlang VM with the
expressiveness of Clojure could provide an interesting, useful result
to make the lives of many programmers simpler and make the world a
happier place.

## Goals

- Interoperability as smooth as possible, just like Clojure proper and
  ClojureScript do.
- Provide most Clojure abstractions.
- Provide all Erlang abstractions and toolset.
- Include a default OTP library in Clojerl.

### Original author's personal goal

The following goal comes from Juan Facorro's original project README:

Learn more about Erlang (and its VM), Clojure and language
implementation.

This project is an experiment that I hope others will find useful.
Regardless of whether it becomes a fully functional implementation of
Clojure or not, I will have learned a lot along the way.

## QAs

### What is Clojerl?

Clojerl is an experimental implementation of Clojure on the Erlang VM.
Its goal is to leverage the features and abstractions of Clojure that
we love (macros, collections, seq, protocols, multimethods, metadata,
etc.), with the robustness the Erlang VM provides for building
(distributed) systems.

### Have you heard about LFE and Joxa?

Yes. LFE and Joxa were each created with very specific and different
goals in mind. LFE was born to provide a LISP syntax for Erlang. Joxa
was mainly created as a platform for creating DSLs that could take
advantage of the Erlang VM. Its syntax was inspired by Clojure but the
creators weren't interested in implementing all of Clojure's features.

### Aren't the language constructs for concurrency very different between Clojure and Erlang?

Yes, they are. On one hand Clojure provides tools to handle mutable
state in a sane way, while making a clear distinction between identity
and state through reference types. On the other, concurrency in the
Erlang VM is implemented through processes and message passing. The
idea in Clojerl is to encourage the Erlang/OTP concurrency model, but
support as many Clojure constructs as possible and as far as they make
sense in the Erlang VM.

### But... but... Rich Hickey lists [here](https://clojure.org/about/state#actors) some of the reasons why he chose not to use the actor model in Clojure.

That is not a question, but I see what you mean :). The points he
makes are of course very good. For example, when no state is shared
between processes there is some communication overhead, but this
isolation is also an advantage under a lot of circumstances. He also
mentions
[here](https://groups.google.com/forum/#!msg/clojure/Kisk_-9dFjE/_2WxSxyd1SoJ) that
building for the distributed case (a.k.a processes and message
passing) is more complex and not always necessary, so he decided to
optimise for the non-distributed case and add distribution to the
parts of the system that need it. Rich Hickey calls Erlang "quite
impressive", so my interpretation of these writings is that they are
more about exposing the rationale behind the decisions and the
trade-offs he made when designing Clojure (on the JVM), than about
disregarding the actor model.

### Will Clojerl support every single Clojure feature?

No. Some of Clojure's features are implemented by relying on the
underlying mutability of the JVM and its object system. The Erlang VM
provides very few mutability constructs and no support for defining
new types. This makes it very hard or nearly impossible to port some
features into Clojerl's implementation.

### Can I reuse existing Clojure(Script) libraries?

Yes, but they will need to be ported, just like for ClojureScript. In
fact, most of Clojure's core namespaces were ported from the original
.clj files in the Clojure JVM repository.

## Support and project history

Report fork-specific bugs and compatibility problems in the
[fork issue tracker][fork-issues].

Credit for Clojerl's language design and original implementation belongs to
[Juan Facorro][original-author] and the
[original contributors][contributors]. Historical discussions, design context,
and prior releases remain available in the [upstream repository][upstream].

[rebar3]: https://github.com/erlang/rebar3
[examples]: scripts/examples
[example-web-app]: https://github.com/clojerl/example-web-app/
[rebar3_clojerl]: https://github.com/clojerl/rebar3_clojerl
[upstream]: https://github.com/clojerl/clojerl
[upstream-docs]: https://www.clojerl.io/
[original-author]: https://github.com/jfacorro
[contributors]: https://github.com/clojerl/clojerl/graphs/contributors
[fork-issues]: https://github.com/antlobach/clojerl/issues
