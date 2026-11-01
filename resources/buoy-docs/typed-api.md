---
title: "binarywriter typed API"
summary: "Source-derived public signatures, properties, parameters, defaults, and return values."
category: "Reference"
order: 21
types: ["BinaryWriter"]
dependencies: []
symbols: ["BinaryWriter"]
related: ["api.md", "getting-started.md", "index.md"]
---

# binarywriter typed API

Generated from the package source declarations. Platform guards are shown where present; inherited members are documented on the base type. See the [API contracts](api.md) for behavior and limitations.

## BinaryWriter

`Public Class BinaryWriter`

Bounded append-only binary encoder with independent output snapshots.

### Properties

| Name | Type | Read-Only | Shared | Default | Description |
| --- | --- | --- | --- | --- | --- |
| `Count` | `Integer` | Yes | No | — | — |
| `Remaining` | `Integer` | Yes | No | — | — |

### Constructor

`Public Sub Constructor(maxBytes As Integer = 16777216)`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `maxBytes` | `Integer` | `ByVal` | `16777216` |

Returns: no value.

### Clear

`Public Sub Clear()`

No parameters.

Returns: no value.

### ToBytes

`Public Function ToBytes() As Bytes`

No parameters.

#### Returns

| Type |
| --- |
| `Bytes` |

### WriteBytes

`Public Sub WriteBytes(data As Bytes)`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `data` | `Bytes` | `ByVal` | Required |

Returns: no value.

### WriteUTF8

`Public Sub WriteUTF8(sourceText As String)`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `sourceText` | `String` | `ByVal` | Required |

Returns: no value.

### WriteUInt8

`Public Sub WriteUInt8(value As Integer)`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `value` | `Integer` | `ByVal` | Required |

Returns: no value.

### WriteInt8

`Public Sub WriteInt8(value As Integer)`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `value` | `Integer` | `ByVal` | Required |

Returns: no value.

### WriteUInt16

`Public Sub WriteUInt16(value As Integer, littleEndian As Boolean = True)`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `value` | `Integer` | `ByVal` | Required |
| `littleEndian` | `Boolean` | `ByVal` | `True` |

Returns: no value.

### WriteInt16

`Public Sub WriteInt16(value As Integer, littleEndian As Boolean = True)`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `value` | `Integer` | `ByVal` | Required |
| `littleEndian` | `Boolean` | `ByVal` | `True` |

Returns: no value.

### WriteUInt32

`Public Sub WriteUInt32(value As Integer, littleEndian As Boolean = True)`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `value` | `Integer` | `ByVal` | Required |
| `littleEndian` | `Boolean` | `ByVal` | `True` |

Returns: no value.

### WriteInt32

`Public Sub WriteInt32(value As Integer, littleEndian As Boolean = True)`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `value` | `Integer` | `ByVal` | Required |
| `littleEndian` | `Boolean` | `ByVal` | `True` |

Returns: no value.

### WriteVarUInt

`Public Sub WriteVarUInt(value As Integer)`

Canonical unsigned LEB128 over the nonnegative Integer range, 0..2^63-1.

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `value` | `Integer` | `ByVal` | Required |

Returns: no value.

### WriteVarInt

`Public Sub WriteVarInt(value As Integer)`

ZigZag followed by unsigned LEB128, for the complete signed Integer range.

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `value` | `Integer` | `ByVal` | Required |

Returns: no value.

### WriteFloat32

`Public Sub WriteFloat32(value As Single, littleEndian As Boolean = True)`

Write IEEE binary32 bits without numeric conversion.

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `value` | `Single` | `ByVal` | Required |
| `littleEndian` | `Boolean` | `ByVal` | `True` |

Returns: no value.

### WriteFloat64

`Public Sub WriteFloat64(value As Double, littleEndian As Boolean = True)`

Write IEEE binary64 bits without numeric conversion.

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `value` | `Double` | `ByVal` | Required |
| `littleEndian` | `Boolean` | `ByVal` | `True` |

Returns: no value.
