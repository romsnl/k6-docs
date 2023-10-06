---
title: 'File.seek'
excerpt: 'File.seek method seeks to the given offset under the mode given by whence'
---

The `File.seek()` method seeks to the given `offset` under the mode given by `whence`. It effectively sets the offset for the next [read](/javascript-api) operation, interpreted according to the value of [`whence`](#parameters). `File.seek()` resolves to the new offset to the start of the file, in bytes.

Seeking to an offset before the start of the file is an error. Seeking to any positive offset is legal.

## Parameters

| Parameter | Type | Description |
| :-------- | :--- | :---------- |
| offset | number | The offset to seek to. |
| whence | [SeekMode](#seekmode) | The mode to use when seeking. |

### SeekMode

`SeekMode` is an enum defining the possible modes to use when seeking. 

| Value | Description |
| :---- | :---------- |
| `Start` | Seek relative to the start of the file. |
| `Current` | Seek relative to the current position. |
| `End` | Seek relative to the end of the file. The `offset` argument to seek should be a negative number when using this mode. |

## Returns 

A [`Promise`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) resolving to the new offset relative to the start of the file (in bytes).

## Example

<CodeGroup labels={["example-fs-seek.js"]} lineNumbers={[]} showCopyButton={[true]}>

```javascript
import { open, SeekMode } from "k6/experimental/fs";

// Files can only be opened in the init context
let file;
(async function () {
	file = await open("bonjour.txt");
})();

export default async function () {
    let offset = 0;

    // Seek 6 bytes from the start of the file
    offset = await file.seek(6, SeekMode.Start);

    // Seek 2 more bytes from the current position
    offset = await file.seek(2, SeekMode.Current);

    // Seek backwards 2 bytes from the end of the file
    offset = await file.seek(-2, SeekMode.End);
}
```

</CodeGroup>