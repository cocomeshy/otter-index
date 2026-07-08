# Otter package registry

The official package index for the Otter programming language. `otter pkg` reads `index.ottix` from this repo to resolve dependencies by name and version.

Otter is a compiled systems language with no garbage collector and no libc dependency (pthread for threading is the one exception); everything else goes through raw syscalls and DLL imports.

## Packages

| Package | Description |
|---------|-------------|
| [otter-array](https://github.com/cocomeshy/otter-array) | Array helpers: search, transform, and aggregate arr<int> |
| [otter-async](https://github.com/cocomeshy/otter-async) | Cooperative async/await runtime (one OS thread per task) |
| [otter-env](https://github.com/cocomeshy/otter-env) | Process environment: env vars, cwd, pid |
| [otter-format](https://github.com/cocomeshy/otter-format) | Formatting helpers for numbers, strings, and epoch timestamps |
| [otter-fs](https://github.com/cocomeshy/otter-fs) | Filesystem access over raw syscalls (read, write, stat, mkdir) |
| [otter-io](https://github.com/cocomeshy/otter-io) | Cross-platform stdout I/O |
| [otter-memory](https://github.com/cocomeshy/otter-memory) | Manual memory allocation and low-level string helpers |
| [otter-os](https://github.com/cocomeshy/otter-os) | Process exit and other OS-level primitives |
| [otter-panic](https://github.com/cocomeshy/otter-panic) | Runtime trap handler for compiler-emitted safety checks |
| [otter-test](https://github.com/cocomeshy/otter-test) | Hard-exit assertions for main()-style test programs |
| [otter-testing](https://github.com/cocomeshy/otter-testing) | Assertions for native test blocks |
| [otter-thread](https://github.com/cocomeshy/otter-thread) | Cross-platform threading and lock-free mutexes |
| [otter-time](https://github.com/cocomeshy/otter-time) | Wall-clock and monotonic time measurement |

`otter-test` and `otter-testing` sound similar but do different things. `test` gives you hard-exit assertions for a plain `main()`-style test program, it calls `os.exit(1)` on the first failure and takes the whole program down with it. `testing` is what backs the native `test "name" { }` block syntax, its assertions throw instead of exiting, so the compiler-generated harness catches each block independently and one failure doesn't stop the rest of the suite from running. Prefer `testing` unless you have a reason not to.

## Using the registry

In your `otter.nest`:

```nest
registry "main" {
  git "https://github.com/cocomeshy/otter-index.git"
  track "stable"
}

deps {
  use "array" want "1.0.0"
  use "thread" want "1.0.0"
}
```

Then run:

```sh
otter pkg pull
```

## Index format

`index.ottix` is a pipe-delimited text file, one line per package version:

```
name|version|git-url|commit|sha256-digest
```

`otter pkg verify` recomputes the digest from the package's entry file and compares it against this index.

## Adding a package here

Each package lives in its own `cocomeshy/otter-<name>` repo with an `otter.rune` manifest and a README generated from its doc comments. Once a package passes `otter pkg check` (documented syscalls, documented exports, working tests), its repo gets pushed and a line is appended to `index.ottix`.
