# 토케이(Tokei, 시계) ([時計](https://en.wiktionary.org/wiki/%E6%99%82%E8%A8%88))
[![Mean Bean CI](https://github.com/XAMPPRocky/tokei/workflows/Mean%20Bean%20CI/badge.svg)](https://github.com/XAMPPRocky/tokei/actions?query=workflow%3A%22Mean+Bean+CI%22)
[![crates.io](https://img.shields.io/crates/d/tokei.svg)](https://crates.io/crates/tokei)
[![Help Wanted](https://img.shields.io/github/issues/XAMPPRocky/tokei/help%20wanted?color=green)](https://github.com/XAMPPRocky/tokei/issues?q=is%3Aissue+is%3Aopen+label%3A%22help+wanted%22)
[![Lines Of Code](https://tokei.rs/b1/github/XAMPPRocky/tokei?category=code)](https://github.com/XAMPPRocky/tokei)
[![Documentation](https://docs.rs/tokei/badge.svg)](https://docs.rs/tokei/)
 
Tokei는 코드에 대한 통계를 표시하는 프로그램입니다. Tokei는 해당 언어별로 파일 수, 해당 파일 및 코드의 총 줄 수, 주석 및 공백수를 표시합니다.

### Translations
- [中文](https://github.com/chinanf-boy/tokei-zh#支持的语言)
- [한글](https://github.com/XAMPPRocky/tokei/READMD-ko.md)

## 예제
```console 
-------------------------------------------------------------------------------
 Language            Files        Lines         Code     Comments       Blanks
-------------------------------------------------------------------------------
 BASH                    4           49           30           10            9
 JSON                    1         1214         1214            0            0
 Markdown                4         1236         1236            0            0
 Rust                   19         3049         2224          431          394
 Shell                   1           49           38            1           10
 TOML                    1           79           68            0           11
-------------------------------------------------------------------------------
 Total                  30         5676         4810          442          424
-------------------------------------------------------------------------------
```

## [도큐먼트 문서](https://docs.rs/tokei)

## 목차

- [특징](#features)
- [설치법](#installation)
    - [패키지 매니저 설치](#package-managers)
    - [수동 설치](#manual)
- [Tokei 이용방법](#how-to-use-tokei)
- [옵션](#options)
- [뱃지](#badges)
- [플러그인](#plugins)
- [지원 언어](#supported-languages)
- [Changelog](CHANGELOG.md)
- [일반적인 이슈들](#common-issues)
- [소스](#canonical-source)
- [저작권 및 라이센스](#copyright-and-license)

## 특징

- Tokei 는 **매우 빠릅니다**, 다른 것들과 비교를 보여주는 [최근 릴리즈](https://github.com/XAMPPRocky/tokei/releases/latest)를 확인해보세요.

- Tokei는 **정확합니다**, Tokei는 여러 줄의 주석, 중첩 된 주석을 바르게 처리하고 문자열에 있는 주석도 계산하지 않습니다. 정확한 코드 통계를 제공합니다.

- Tokei는 **150개** 이상의 광범위한 언어와 다양한 확장을 지원있습니다.

- Tokei는 여러 형식 (**CBOR**, **JSON**, **TOM**, **YAML**)으로 출력 할 수 있으며 Tokei의 출력을 쉽게 저장, 재사용 할 수 있습니다. 
  이전 실행의 통계를 다른 세트와 결합하여 tokei에서 재사용 할 수도 있습니다.

- Tokei는 **Mac**, **Linux**, **Windows** 에서 사용가능합니다. OS별로 어떻게 설치하는지 [설치법]을 참고해주세요.

- Tokei is also a **library** allowing you to easily integrate it with other
  projects.

## Installation

### Package Managers
```console
# Arch Linux
pacman -S tokei
# Cargo
cargo install tokei
# Conda
conda install -c conda-forge tokei
# Fedora
sudo dnf install tokei
# FreeBSD
pkg install tokei
# MacOS (Homebrew)
brew install tokei
# Nix/NixOS
nix-env -i tokei
# OpenSUSE
sudo zypper install tokei
```

### Manual

#### Downloading
You can download prebuilt binaries in the
[releases section](https://github.com/XAMPPRocky/tokei/releases).

#### Building
You can also build and install from source (requires the latest stable [Rust] compiler.)
```console
cargo install --git https://github.com/XAMPPRocky/tokei.git
```

[rust]: https://www-rust-lang.org

## How to use Tokei

#### Basic usage

This is the basic way to use tokei. Which will report on the code in `./foo`
and all subfolders.

```shell
$ tokei ./foo
```

#### Multiple folders
To have tokei report on multiple folders in the same call simply add a comma,
or a space followed by another path.

```shell
$ tokei ./foo ./bar ./baz
```
```shell
$ tokei ./foo, ./bar, ./baz
```

#### Excluding folders
Tokei will respect all `.gitignore` and `.ignore` files, and you can use
the `--exclude` option to exclude any additional files. The `--exclude` flag has
the same semantics as `.gitignore`.

```shell
$ tokei ./foo --exclude *.rs
```

#### Sorting output
By default tokei sorts alphabetically by language name, however using `--sort`
tokei can also sort by any of the columns.

`blanks, code, comments, lines`

```shell
$ tokei ./foo --sort code
```

#### Outputting file statistics
By default tokei only outputs the total of the languages, and using `--files`
flag tokei can also output individual file statistics.

```shell
$ tokei ./foo --files
```

#### Outputting into different formats
Tokei normally outputs into a nice human readable format designed for terminals.
There is also using the `--output` option various other formats that are more
useful for bringing the data into another program.

**Note:** This version of tokei was compiled without any serialization formats, to enable serialization, reinstall
tokei with the features flag.

```shell
  ALL:
  cargo install tokei --features all

  JSON:
  cargo install tokei --features json

  CBOR:
  cargo install tokei --features cbor

  YAML:
  cargo install tokei --features yaml

  TOML:
  cargo install tokei --features toml
```

**Currently supported formats**
- JSON `--output json`
- YAML `--output yaml`
- TOML `--output toml`
- CBOR `--output cbor`

```shell
$ tokei ./foo --output json
```

#### Reading in stored formats
Tokei can also take in the outputted formats added in the previous results to it's
current run. Tokei can take either a path to a file, the format passed in as a
value to the option, or from stdin.

```shell
$ tokei ./foo --input ./stats.json
```

## Options

```
USAGE:
    tokei [FLAGS] [OPTIONS] [--] [input]...

FLAGS:
    -f, --files               Will print out statistics on individual files.
    -h, --help                Prints help information
        --hidden              Count hidden files.
    -l, --languages           Prints out supported languages and their extensions.
        --no-ignore           Don't respect ignore files.
        --no-ignore-parent    Don't respect ignore files in parent directories.
        --no-ignore-vcs       Don't respect VCS ignore files.
    -V, --version             Prints version information
    -v, --verbose             Set log output level:
                                          1: to show unknown file extensions,
                                          2: reserved for future debugging,
                                          3: enable file level trace. Not recommended on multiple files

OPTIONS:
    -c, --columns <columns>       Sets a strict column width of the output, only available for terminal output.
    -e, --exclude <exclude>...    Ignore all files & directories containing the word.
    -i, --input <file_input>      Gives statistics from a previous tokei run. Can be given a file path, or "stdin" to
                                  read from stdin.
    -o, --output <output>         Outputs Tokei in a specific format. Compile with additional features for more format
                                  support. [possible values: cbor, json, yaml]
    -s, --sort <sort>             Sort languages based on column [possible values: files, lines, blanks, code, comments]
    -t, --type <types>            Filters output by language type, seperated by a comma. i.e. -t=Rust,Markdown

ARGS:
    <input>...    The input file(s)/directory(ies) to be counted.
```

## Badges
Tokei has support for badges. For example
[![](https://tokei.rs/b1/github/XAMPPRocky/tokei)](https://github.com/XAMPPRocky/tokei).

```
[![](https://tokei.rs/b1/github/XAMPPRocky/tokei)](https://github.com/XAMPPRocky/tokei).
```

Tokei's URL scheme is as follows.

```
https://tokei.rs/b1/{host: values: github|gitlab}/{Repo Owner eg: XAMPPRocky}/{Repo name eg: tokei}
```

By default the badge will show the repo's LoC(_Lines of Code_), you can also
specify for it to show a different category, by using the `?category=` query
string. It can be either `code`, `blanks`, `files`, `lines`, `comments`,
Example show total lines:

```
[![](https://tokei.rs/b1/github/XAMPPRocky/tokei?category=lines)](https://github.com/XAMPPRocky/tokei).
```

The server code hosted on tokei.rs is in [XAMPPRocky/tokei_rs](https://github.com/XAMPPRocky/tokei_rs)

## Plugins
Thanks to contributors tokei is now available as a plugin for some text editors.

- [Vim](https://github.com/vmchale/tokei-vim) by [vmchale](https://github.com/vmchale/)

## Supported Languages

If there is a language that you would to add to tokei feel free to make a pull
request. Languages are defined in [`languages.json`](./languages.json), and you can
read how to add and test your language in our [CONTRIBUTING.md](./CONTRIBUTING.md).

```
Abap
ActionScript
Ada
Agda
Alex
Asn1
Asp
AspNet
Assembly
AssemblyGAS
Autoconf
AutoHotKey
Automake
Bash
Batch
BrightScript
C
Cabal
Cassius
Ceylon
CHeader
Clojure
ClojureC
ClojureScript
CMake
Cobol
CoffeeScript
Cogent
ColdFusion
ColdFusionScript
Coq
Cpp
CppHeader
Crystal
CSharp
CShell
Css
D
Dart
DeviceTree
Dockerfile
DotNetResource
DreamMaker
Dust
Edn
Elisp
Elixir
Elm
Elvish
EmacsDevEnv
Emojicode
Erlang
FEN
Fish
FlatBuffers
Forth
FortranLegacy
FortranModern
FreeMarker
FSharp
Fstar
GDB
GdScript
Gherkin
Glsl
Go
Graphql
Groovy
Hamlet
Handlebars
Happy
Haskell
Haxe
Hcl
Hex
Hlsl
HolyC
Html
Idris
Ini
IntelHex
Isabelle
Jai
Java
JavaScript
Json
Jsx
Julia
Julius
KakouneScript
Kotlin
Lean
Less
LinkerScript
Liquid
Lisp
LLVM
Logtalk
Lua
Lucius
Madlang
Makefile
Markdown
Meson
Mint
ModuleDef
MoonScript
MsBuild
Mustache
Nim
Nix
NotQuitePerl
ObjectiveC
ObjectiveCpp
OCaml
Odin
Org
Oz
Pascal
Perl
Perl6
Pest
Php
Polly
Pony
PostCss
PowerShell
Processing
Prolog
Protobuf
PSL
PureScript
Python
Qcl
Qml
R
Racket
Rakefile
Razor
Renpy
ReStructuredText
RON
RPMSpecfile
Ruby
RubyHtml
Rust
Sass
Scala
Scheme
Scons
Sh
Sml
Solidity
SpecmanE
Spice
Sql
SRecode
Stratego
Svg
Swift
Swig
SystemVerilog
Tcl
Tex
Text
Thrift
Toml
Twig
TypeScript
UnrealDeveloperMarkdown
UnrealPlugin
UnrealProject
UnrealScript
UnrealShader
UnrealShaderHeader
UrWeb
UrWebProject
Vala
VB6
VBScript
Velocity
Verilog
VerilogArgsFile
Vhdl
VimScript
VisualBasic
VisualStudioProject
VisualStudioSolution
Vue
WebAssembly
Wolfram
Xaml
XcodeConfig
Xml
XSL
Xtend
Yaml
Zig
Zsh
```

## Common issues

### Tokei says I have a lot of D code, but I know there is no D code!
This is likely due to `gcc` generating `.d` files. Until the D people decide on
a different file extension, you can always exclude `.d` files using the
`-e --exclude` flag like so

```
$ tokei . -e *.d
```

## Canonical Source
이 레포지터리 소스는 [GitHub](https://github.com/XAMPPRocky/tokei)에 호스팅 되어있습니다. 
깃허브에 계정이 있으시면 이슈나 풀리퀘스트를 보내주세요

## Copyright and License
(C) Copyright 2015 by XAMPPRocky and contributors

See [the graph](https://github.com/XAMPPRocky/tokei/graphs/contributors) for a full list of contributors.

Tokei is distributed under the terms of both the MIT license and the Apache License (Version 2.0).

See [LICENCE-APACHE](./LICENCE-APACHE), [LICENCE-MIT](./LICENCE-MIT) for more information.
