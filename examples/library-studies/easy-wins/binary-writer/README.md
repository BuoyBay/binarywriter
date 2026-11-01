# Binary record writing

This example encodes a four-byte magic value, little-endian version, big-endian
32-bit value and unsigned/ZigZag varints. It prints `42554f5901000000002aac02ab02`.

From this example directory, install its source dependency and run:

```sh
bpm install
mkdir -p dist
bpm build main.bui -o dist/example
./dist/example
```

BPM locks the resolved package commit. The package is acquired from its own
repository; generated package archives stay local. Read the
[package contract](../../../../docs/API.md) for bounds and encoding rules.
