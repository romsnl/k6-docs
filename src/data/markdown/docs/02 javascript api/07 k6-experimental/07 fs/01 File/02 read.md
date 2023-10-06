---
title: 'File.read'
excerpt: 'File.read data'
---

The `File.read()` method is used to read a chunk of the file into an array buffer. 

## Parameters

| Parameter | Type | Description |
| :-------- | :--- | :---------- |
| buffer | [Uint8Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Uint8Array) | The buffer into which the data will be read. **It is not guaranteed that the full buffer will be read in a single call.** |

## Returns 

A [`Promise`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) resolving to the number of bytes read or `null` (EOF) if there was nothing more to read.

## Example

<CodeGroup labels={["example-fs-read.js"]} lineNumbers={[]} showCopyButton={[true]}>

```javascript
import { open, SeekMode } from "k6/experimental/fs";

// Open a file in the init context
let file;
(async function () {
	file = await open("bonjour.txt");
})();

export default async function () {
    // Initialize a buffer to read into
	const buffer = new Uint8Array(4);

	let offset = 0;
	while (true) {
		// Read a chunk of the file into the buffer
		const bytesRead = await file.read(buffer);
		if (bytesRead == null) {
			// EOF
			break;
		}

		// Do something useful with the content of the buffer

		offset += bytesRead;

		// If bytesRead is less than the buffer size, we've read the whole file
		if (bytesRead < buffer.byteLength) {
			break;
		}
	}

	// Seek back to the beginning of the file
	await file.seek(0, SeekMode.Start);
}
```

</CodeGroup>