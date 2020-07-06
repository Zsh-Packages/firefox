# Firefox Browser as a Zsh package

##### Homepage link: [Mozilla Firefox](https://www.mozilla.org/en-US/firefox/)

| **Package source:** | Source Tarball | Binary | Git | Node | Gem |
|:-------------------:|:--------------:|:------:|:---:|:----:|:---:|
| **Status:**         |  -             | + <br> (default) |  -  |   –  |  –  |

[Zinit](https://github.com/zdharma/zinit) can use the `package.json` file to automatically:

- get the plugin's Git repository OR release-package URL,
- get the list of the recommended ices for the plugin,
    - there can be multiple lists of ices,
    - the ice lists are stored in *profiles*; there's at least one profile, *default*,
    - the ices can be selectively overriden.

Example invocations that'll install [Mozilla Firefox](https://www.mozilla.org/en-US/firefox/)

```zsh
# Download the binary of Firefox.
# Supports Windows (Cygwin), Linux and OS X.
zinit pack for firefox-browser

# Download the Firefox binary and set it up
# with use of the Bin-Gem-Node annex.
zinit pack"bgn" for firefox-browser
```

## Default Profile

Provides the CLI commands `firefox-bin` and `firefox` by extending the `$PATH`
to point to the snippet's directory.

The Zinit command executed will be equivalent to:

```zsh
zinit id-as"firefox-browser" as"command" lucid" \
    atclone'local ext="${${${(M)OSTYPE#linux*}:+tar.bz2}:-dmg}"; \
        zpextract %ID% $ext --norm ${${OSTYPE:#darwin*}:+--move}'
    pick"firefox(|-bin)" atpull"%atclone" nocompile is-snippet for \
        "https://download.mozilla.org/?product=firefox-latest-ssl&os=${${${(M)OSTYPE##linux}:+linux64}:-${${(M)OSTYPE##darwin}:+osx}}&lang=en-US"
```

## Bin-Gem-Node Profile

Provides the CLI command `firefox` by creating a forwarder script (a *shim*) to
the `firefox-bin` command, in `$ZPFX/bin` by using the
[Bin-Gem-Node](https://github.com/zinit/z-a-bin-gem-node) annex. It's the best
method of providing the binary to the command line.

The Zinit command executed will be equivalent to:

```zsh
zinit id-as"firefox-browser" as"null" lucid \
    atclone'local ext="${${${(M)OSTYPE#linux*}:+tar.bz2}:-dmg}"; \
        zpextract %ID% $ext --norm ${${OSTYPE:#darwin*}:+--move}'
    atpull"%atclone" nocompile is-snippet for \
        "https://download.mozilla.org/?product=firefox-latest-ssl&os=${${${(M)OSTYPE##linux}:+linux64}:-${${(M)OSTYPE##darwin}:+osx}}&lang=en-US"
```

