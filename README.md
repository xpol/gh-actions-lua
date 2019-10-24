# xpol/install-lua

[![Actions Status](https://github.com/xpol/install-lua/workflows/test/badge.svg)](https://github.com/xpol/install-lua/actions)

Github Action to install Lua/LuaJIT on Unbutu/macOS/Windows.

Builds and installs Lua into the `.lua/` directory in the working directory using [CMake](https://cmake.org/).
Adds the `.lua/bin` to the `PATH` environment variable so `lua` can be called
directly in workflows.

This project is hightlly inspired by (and initial code copied from) [leafo/gh-actions-lua](https://github.com/leafo/gh-actions-lua). The difference is this project use CMake to support Unbutu/macOS/Windows.

Many thanks to [leafo](https://github.com/leafo)!

## Usage

Install Lua:

*Will install the latest stable release of PUC-Rio Lua.*

```yaml
- uses: xpol/install-lua@v1
```

Install specific version of Lua:

```yaml
- uses: xpol/install-lua@v1
  with:
    luaVersion: "5.1.5"
```

Install specific version of LuaJIT:

```yaml
- uses: xpol/install-lua@v1
  with:
    luaVersion: "luajit-2.0.5"
```

## Inputs

### `luaVersion`

**Default**: `"5.3.5"`

Specifies the version of Lua to install. The version name instructs the action
where to download the source from.

All supported of versions:

* `"5.4.0-beta1"`
* `"5.3.5"`
* `"5.3.4"`
* `"5.3.3"`
* `"5.3.2"`
* `"5.3.1"`
* `"5.3.0"`
* `"5.2.4"`
* `"5.2.3"`
* `"5.2.2"`
* `"5.2.1"`
* `"5.2.0"`
* `"5.1.5"`
* `"5.1.4"`
* `"5.1.3"`
* `"5.1.2"`
* `"5.1.1"`
* `"5.1.0"`
* `"5.0.3"`
* `"5.0.2"`
* `"5.0.1"`
* `"5.0.0"`
* `"luajit-2.0.5"`
* `"luajit-2.1.0-beta3"`
* `"luajit-2.1.0-beta2"`
* `"luajit-2.1.0-beta1"`
* `"luajit-2.0.4"`
* `"luajit-2.0.3"`
* `"luajit-2.0.2"`
* `"luajit-2.0.1"`
* `"luajit-2.0.0"`

*Note: Beta versions will removed when the stable version is released.*

**Version aliases:**

You can use shorthand `"5.1"`, `"5.2"`, `"5.3"`, `"luajit"` version aliases to point to the
latest stable version of Lua for that version.

## Also want LuaRocks

You may also need an action to install luarocks:

* [`leafo/gh-actions-luarocks`](https://github.com/leafo/gh-actions-luarocks)
  * inputs: `luarocksVersion`

## Full Example

This example is for running tests on a Lua module that uses LuaRocks for
dependencies and [busted](https://olivinelabs.com/busted/) for a test suite.

Create `.github/workflows/test.yml` in your repository:

```yaml
name: test

on: [push]

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@master

    - uses: xpol/install-lua@v1
      with:
        luaVersion: "5.1.5"

    - uses: leafo/gh-actions-luarocks@v2

    - name: build
      run: |
        luarocks install busted
        luarocks make

    - name: test
      run: |
        busted -o utfTerminal
```

This example:

* Uses Lua 5.1.5 — You can use another version by chaning the `luaVersion` varible. LuaJIT versions can be used by prefixing the version with `luajit-`, i.e. `luajit-2.1.0-beta3`
* Uses a `.rockspec` file the root directory of your repository to install dependencies and test packaging the module via `luarocks make`

View the documentation for the individual actions (linked above) to learn more about how they work.

### Version build matrix

You can test against multiple versions of Lua using a matrix strategy:

```yaml
jobs:
  test:
    strategy:
      matrix:
        luaVersion: ["5.1.5", "5.2.4", "luajit-2.1.0-beta3"]

    steps:
    - uses: actions/checkout@master
    - uses: xpol/install-lua@v1
      with:
        luaVersion: ${{ matrix.luaVersion }}

    # ...
```
