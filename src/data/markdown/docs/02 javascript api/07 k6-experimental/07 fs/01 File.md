---
title: 'File'
excerpt: 'File is an abstraction to interact with files which exposes read-only operations.'
---

`File` is an abstraction to interaction with files and exposes read-only operations on files. It is obtained as a result of calling the [`open()`](/javascript-api/k6-experimental/fs/open) function.

It provides a standardized way to read the content of a file, seek through it, get information about it, and close it. 

## Methods

| Method                                                | Description                                   |
| :---------------------------------------------------  | :-------------------------------------------- |
| [read](/javascript-api/k6-experimental/fs/file/file-read)  | reads a chunk of the file into an [ArrayBuffer](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/ArrayBuffer).      |
| [stat](/javascript-api/k6-experimental/fs/file/file-stat)  | returns information about the `File`.         |
| [seek](/javascript-api/k6-experimental/fs/file/file-seek)  | seek to a given offset within the file.       |
| [close](/javascript-api/k6-experimental/fs/file/file-close) | closes the file.                              |

## Example

<CodeGroup labels={["example-fs-file.js"]} lineNumbers={[]} showCopyButton={[true]}>

```javascript
import { open, SeekMode } from "k6/experimental/fs";

// Files can only be opened in the init context
let file;
(async function () {
	file = await open("bonjour.txt");
})();

export default async function () {
	// Access information about the file using `File.stat()`
	const fileinfo = await file.stat();
	if (fileinfo.name != "bonjour.txt") {
		throw new Error("Unexpected file name");
	}


	const buffer = new Uint8Array(4);
	let totalBytesRead = 0;
	while (true) {
		// Read chunks of the file into the buffer using `File.read()`
		const bytesRead = await file.read(buffer);
		if (bytesRead == null) {
			// EOF
			break;
		}

		totalBytesRead += bytesRead;

		// If bytesRead is less than the buffer size, we've read the whole file
		if (bytesRead < buffer.byteLength) {
			break;
		}
	}

	// Seek back to the beginning of the file using `File.seek()`
	await file.seek(0, SeekMode.Start);
}
```

</CodeGroup>