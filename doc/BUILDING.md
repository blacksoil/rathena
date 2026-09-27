# Building rAthena

## Mac OSX

Building on Apple Silicon (M-series) macOS with Homebrew can fail with:

```
npc_chat.cpp:8:10: fatal error: 'pcre.h' file not found
```

This happens because:

1. `./configure` was never run, so no `Makefile.inc`/generated `Makefile`s exist yet.
2. PCRE isn't installed via Homebrew.
3. Even after installing PCRE, `configure`'s default detection only adds `-lpcre`
   for linking — it doesn't add an include path unless `PCRE_HOME` is set. This
   matters on Apple Silicon because Homebrew installs to `/opt/homebrew`, which
   is not on the compiler's default include search path (unlike `/usr/local` on
   Intel Macs).

To fix it, install PCRE and re-run `configure` with `PCRE_HOME` pointing at your
Homebrew prefix:

```sh
brew install pcre
PCRE_HOME=/opt/homebrew ./configure
make server
```

Note: Homebrew's `pcre` formula is deprecated upstream in favor of `pcre2`, but
rAthena's build system still expects the original PCRE library, so install
`pcre`, not `pcre2`.

If you ever need to re-run `configure` (e.g. after pulling changes that touch
`configure.ac`), remember to keep passing `PCRE_HOME=/opt/homebrew`, otherwise
this same error will resurface.
