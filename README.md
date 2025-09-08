# @toonamfjs/bytearray-node

[![npm version](https://img.shields.io/npm/v/bytearray-node?style=flat-square)](https://www.npmjs.com/package/bytearray-node)

A Node.js implementation of an ActionScript 3-style `ByteArray` providing a convenient, high-level API for reading and writing binary data streams, with support for various primitive types, strings, compression, and buffer management, based on bytearray-node by Zaseth.

This project is also a forked version from [`bytearray-node`](https://github.com/Pyrodash/bytearray-node) from [@Pyrodash](https://github.com/Pyrodash).

## Features

- **Read and write primitive types:**
    - Signed/unsigned bytes, shorts, ints, floats, doubles, booleans
- **String support:**  
    - Read/write UTF-8 strings and multi-byte strings with custom character sets (using [iconv-lite](https://www.npmjs.com/package/iconv-lite))
- **Buffer stream API:**  
    - Maintain a movable position within the buffer like a stream
    - `bytesAvailable` to check remaining unread bytes
    - Initialize from `Array` or `Buffer`
- **Compression and decompression:**  
    - Built-in support for `zlib` and `deflate` compression/decompression via Node.js
- **Buffer management:**  
    - Dynamically expands the buffer as needed
    - Clear, reset, or convert buffer to JSON or string

## Installation

```bash
$ npm install @toonamf/bytearray-node`
```

## Usage

```javascript
const ByteArray = require('@toonamfjs/bytearray-node')
const ba = new ByteArray()

// Write various types
ba.writeByte(1)
ba.writeShort(5)
ba.writeInt(123456)
ba.writeDouble(Math.PI)
ba.writeUTF('Hello, world!')

// Reset position for reading
ba.position = 0

console.log(ba.readByte())    // 1
console.log(ba.readShort())   // 5
console.log(ba.readInt())     // 123456
console.log(ba.readDouble())  // 3.141592653589793
console.log(ba.readUTF())     // Hello, world!
```

## API Highlights

### Construction

```javascript
new ByteArray()                // Empty buffer
new ByteArray([1,2,3])         // From array
new ByteArray(Buffer.alloc(4)) // From Buffer
```

### Reading and Writing

- `writeByte(value)`, `readByte()`
- `writeUnsignedByte(value)`, `readUnsignedByte()`
- `writeShort(value)`, `readShort()`
- `writeUnsignedShort(value)`, `readUnsignedShort()`
- `writeInt(value)`, `readInt()`
- `writeUnsignedInt(value)`, `readUnsignedInt()`
- `writeFloat(value)`, `readFloat()`
- `writeDouble(value)`, `readDouble()`
- `writeBoolean(value)`, `readBoolean()`
- `writeUTF(string)`, `readUTF()`
- `writeUTFBytes(string)`, `readUTFBytes(length)`
- `writeMultiByte(string, charset)`, `readMultiByte(length, charset)`
- `writeBytes(otherByteArray[, offset[, length]])`, `readBytes(otherByteArray[, offset[, length]])`

### Compression

- `compress(algorithm)`  
  Compresses the buffer (`algorithm` = `'zlib'` or `'deflate'`)

- `uncompress(algorithm)`  
  Decompresses the buffer

### Buffer Management

- `clear()`  
  Clears the buffer and resets position

- `reset(buffer)`  
  Re-initializes from an array or buffer

- `toJSON()`, `toString()`  
  Convert buffer to JSON or string

- Properties:  
    - `position` (current read/write position)
    - `length` (buffer length; can be set)
    - `bytesAvailable` (remaining bytes to read)
    - `endian` (endianness; true = BE, false = LE)

## Example: Buffer Expansion & Compression

```javascript
const ba = new ByteArray()
ba.writeUTFBytes('Hello, ByteArray!')
ba.compress('zlib')
ba.uncompress('zlib')
ba.position = 0
console.log(ba.readUTFBytes(17)) // Hello, ByteArray!
```

---

## License

MIT License © 2025 Juansecu

---

> **Note:**  
> While the API is inspired by ActionScript 3's ByteArray, this package does not provide AMF0/AMF3 serialization/deserialization out of the box.
>
> For AMF0/AMF3 serialization/deserialization, consider using `@toonamfjs/client` for client-side and `@toonamfjs/server` for server-side.
