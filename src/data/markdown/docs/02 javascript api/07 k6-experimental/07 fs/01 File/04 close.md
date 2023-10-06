---
title: 'File.close'
excerpt: 'File.close closes a file which has been previously opened'
---

The `File.close()` method closes the file it is called on and frees up the backing resources. It returns a [`Promise`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) resolving when the operation is over.

Closing a file more than once is an error.

## Returns 

A [`Promise`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) resolving once the operation is over.

## Example

<CodeGroup labels={["example-fs-stat.js"]} lineNumbers={[]} showCopyButton={[true]}>

```javascript
import { open } from "k6/experimental/fs";

// Files can only be opened in the init context
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

    // Close the file once we've read its whole content
    if (offset == await file.stat().size()) {
        await file.close();
    }
}
```

</CodeGroup>