# Otter Package Registry

The official package index for the Otter programming language.

## Packages

| Package | Description |
|---------|-------------|
| `memory` | Memory allocation, strings, raw pointer ops |
| `io` | Cross-platform stdout/stdin |
| `async` | Cooperative async runtime |
| `array` | Array utilities (contains, find, reverse, sum, min, max) |
| `thread` | Cross-platform threading and mutexes |

## Usage

In your `otter.nest`:
```
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
```
otter pkg pull
```
