---
title: 'File.stat'
excerpt: 'File.stat reads the metadata of a file metadata'
---

The `File.stat()` method reads a file's metadata.

## Returns 

A [`Promise`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) resolving to a [`FileInfo`](#fileinfo) object for the file.

### FileInfo

The FileInfo object provides information about a file.

| Property | Type | Description |
| :------- | :--- | :---------- |
| `name` | `string` | The [base name](https://en.wikipedia.org/wiki/Basename) of the file. |
| `size` | `number` | The size of the file in bytes. |

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
	// Access information about the file using file's `stat()` method.
	const fileinfo = await file.stat();
	if (fileinfo.name != "bonjour.txt") {
		throw new Error("Unexpected file name");
	}
}
```

</CodeGroup>