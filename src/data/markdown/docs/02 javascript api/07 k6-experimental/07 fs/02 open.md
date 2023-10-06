---
title: 'open'
excerpt: 'open opens a file and resolve to an instance of a File.'
---

<ExperimentalBlockquote />

The `open()` function opens a file and returns a Promise resolving to a [`File`]() instance

Because the k6 [init context](/using-k6/test-lifecycle) does not support using `await` yet, to use this function, users must momentarily use a workaround:

```javascript
import { open } from 'k6/experimental/fs';

let file;
(async function() {
    file = await open("bonjour.txt");
}());

export default async function () { /* ... */ }
```

The use of the `open()` function is limited to the [init context](/using-k6/test-lifecycle). It cannot be used in the default function. The `File` instance returned by `open()` can be however used in the default function. The `open` function is optimized to limit the memory usage of the k6 process (compared to the native [open](/javascript-api/init-context/open) function).

## Parameters

| Name | Type | Description |
| :--- | :--- | :---------- |
| path | string | The path to the file to open. The path should be relative to the script file it called in. |

## Return Value

The `open()` function returns a Promise resolving to a [`File`](/javascript-api/k6-experimental/fs/file) instance.

## Throws

| Type                 | Description                                                              |
| :------------------- | :----------------------------------------------------------------------- |
| ForbiddenError     | Thrown when the `open()` function is called outside of the init context. |
| TypeError          | Thrown when the `path` parameter is invalid.                             |

## Example

<CodeGroup labels={["example-fs-open.js"]} lineNumbers={[]} showCopyButton={[true]}>

```javascript
import { open } from 'k6/experimental/fs';

// Files can only be opened in the init context
let file;
(async function() {
    file = await open("bonjour.txt");
}());

export default function () {
    // Use the file in the default function
    const fileInfo = file.stat();
}
```

</CodeGroup>