# BinaryWriter 0.1.0

`Import binarywriter` provides bounded append-only encoding to owned `Bytes`.
It complements BinaryCursor's exact in-memory reads. BufferKit already supplies
memory-to-file spooling; this package supplies typed binary encoding and does not
introduce another file buffer. It has no package dependencies; float bit copies use the system C runtime.

## API

| Operation | Contract |
| --- | --- |
| `New BinaryWriter(maxBytes = 16777216)` | Capacity limit in 0..16 MiB; initially empty |
| `Count`, `Remaining` | Current output byte count and unused configured capacity |
| `Clear()` | Reset output without changing the limit |
| `ToBytes()` | Independent snapshot; caller mutation cannot change output |
| `WriteBytes(data)` | Copy exact bytes including NUL; Nil rejects |
| `WriteUTF8(sourceText)` | Encode String to UTF-8 bytes; no terminator, BOM or length prefix |
| `WriteUInt8(value)`, `WriteInt8(value)` | 0..255 or -128..127 |
| `WriteUInt16(value, littleEndian = True)`, `WriteInt16(...)` | 0..65535 or -32768..32767 |
| `WriteUInt32(value, littleEndian = True)`, `WriteInt32(...)` | 0..4294967295 or -2147483648..2147483647 |
| `WriteFloat32(value As Single, littleEndian = True)` | IEEE binary32 from Single; exact bits |
| `WriteFloat64(value As Double, littleEndian = True)` | IEEE binary64 from Double; exact bits |
| `WriteVarUInt(value)` | Canonical unsigned LEB128, 0..9223372036854775807 |
| `WriteVarInt(value)` | Full signed Integer range, ZigZag then canonical unsigned LEB128 |

Fixed-width signed values use two's complement. Byte order is explicit for
16/32-bit integer and 32/64-bit floating-point operations and has no effect on varints. A zero value occupies one
varint byte. Unsigned varints use at most nine bytes; signed ZigZag values use
at most ten bytes, including the minimum Integer. Signed varints are the
[Protocol Buffers **sint64** convention](https://protobuf.dev/programming-guides/encoding/#signed-integers), not its `int64` negative-number convention
and not signed LEB128. Examples: 300 unsigned is `ac02`, -150 ZigZag is `ab02`,
and -1 ZigZag is `01`.

Each operation validates range and remaining capacity before changing output.
Varints are staged completely before appending. Invalid arguments/capacity
throw `InvalidArgumentException` without changing previously written bytes.
Empty byte/text writes are valid, including with a zero limit. Supplied Bytes
are copied, so later caller mutation cannot affect output. There is no seek,
patching or shared writable view. Instances are single-threaded.

Failure preserves output for the failing call only. If a record takes several
writes, successful earlier fields remain in the writer after a later field
fails. To append a record as one operation, build it in a separate bounded
writer, then pass its `ToBytes()` snapshot to the destination’s `WriteBytes`.
The final append checks the destination’s remaining capacity before changing
it. `Clear()` discards all output, including any earlier complete records; it
is not a rollback to the start of the failed record.

Capacity bounds committed output, not total process memory: UTF-8 conversion
materializes an input buffer before the capacity check, snapshots allocate a
copy, and Bytes may reserve extra backing capacity. Allocation failure is not a
recoverable transactional contract. String encoding follows Buoy's current
String behavior; use WriteBytes for arbitrary binary data and embedded NUL.

The writer has no file I/O, asynchronous operations or full unsigned-64 API.
Matching canonical varint readers are available in
[BinaryCursor](https://github.com/BuoyBay/binarycursor).

## IEEE floating-point fields

`ReadFloat32` / `WriteFloat32` use `Single` (IEEE binary32);
`ReadFloat64` / `WriteFloat64` use `Double` (IEEE binary64). Both default
to little-endian and accept `False` for big-endian. They copy the value's
bits without arithmetic, preserving signed zero, subnormals, infinities,
and NaN sign/payload bits. Keep values in their original type for exact
roundtrips: numeric conversion or arithmetic by the caller can alter NaNs
or round values. No finite-only policy is imposed; format consumers such as
GLTF must enforce their own validity rules.

Reads require all 4/8 bytes before advancing Position. Writes check the
entire 4/8-byte capacity before changing output. Truncated reads or
insufficient writer capacity throw `InvalidArgumentException` without
changing Position or output, respectively.

Implementation uses `memcpy` from the system C runtime (`ucrtbase` on
Windows), with no extra shipped native library or Xojo dependency. The six
supported desktop targets are little-endian Intel/ARM64; byte order on
the wire remains explicit.
