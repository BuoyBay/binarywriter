# Endian writes and canonical varints

`Import binarywriter` provides bounded append-only encoding to owned `Bytes`.
It complements BinaryCursor's exact in-memory reads. BufferKit already supplies
memory-to-file spooling; this package supplies typed binary encoding and does not
introduce another file buffer. It has no package dependencies; float bit copies use the system C runtime.

## Install

From a Buoy consumer project with BPM installed:

```sh
bpm add binarywriter --git https://github.com/BuoyBay/binarywriter.git --rev refs/heads/main
```

BPM records the resolved commit in its lockfile. Keep that lockfile with the
consumer project for reproducible installs.

## API at a glance

| Operation | Contract |
| --- | --- |
| `New BinaryWriter(maxBytes = 16777216)` | Capacity limit in 0..16 MiB; initially empty |
| `Count`, `Remaining` | Current output byte count and unused configured capacity |
| `Clear()` | Reset output without changing the limit |
| `ToBytes()` | Independent snapshot; caller mutation cannot change output |
| `WriteBytes(data)` | Copy exact bytes including NUL; Nil rejects |
| `WriteUTF8(sourceText)` | Encode String to UTF-8 bytes; no terminator, BOM or length prefix |

## Build from source

With the native `build-packages` driver from [devtools](https://github.com/BuoyBay/devtools)
on your `PATH`, run from this checkout:

```sh
build-packages --root "$PWD" --compile-consumer binarywriter
```

The driver checks source targets and compiles an import consumer. See the
[API guide](docs/API.md) for behavior, limits and any host prerequisites.

## Local Buoy setup

On your own machine, install Buoy and run:

```sh
buoy ai init --both
```


## Contributing

[Contributing](CONTRIBUTING.md) · [License](LICENSE)
