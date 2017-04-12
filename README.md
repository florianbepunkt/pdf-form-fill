# pdf-form-fill

Simple PDF form fill, using [PDFtk]. Uses field data in `xfdf` format to help guarantee UTF-8 support.

All operations are non-blocking. Written in a rather ES6-ish style; use on Node 5.x and earlier will require transpiling.

Release as open source under the [ISC license].


### Installation

1. Make sure `pdftk` is available in your environment, preferably at least version 2.02.
2. `npm install pdf-form-fill`


### Usage

The API is minimal:
- `fields(pdf)` will return a [Promise] resolving with a simple object describing the PDF's available fields.
- `fill(pdf, data[, options])` will return a promise resolving with a [Readable sream] of the output PDF.
  `options` only supports one option, `flatten`, which defaults to `true`

For more details, read the [source code](index.js).


### Example

```js
const fs = require('fs')
const { fields, fill } = require('pdf-form-fill')

const srcPdf = '...'
const tgtPdf = '...'
const data = { name1: 'Value 1', checkbox2: 'Yes' }

fields(srcPdf)
  .then(shape => console.log(shape))
  .catch(err => console.error(err))

const output = fs.createWriteStream(tgtPdf)
fill(scrPdf, data)
  .then(stream => stream.pipe(output))
  .catch(err => console.error(err))
```


### Notes

This tool is the product of frustration and incredulity, as none of the other PDF form-filling tools appeared to
work for my particular use case, requiring UTF-8 support in a macOS environment. For some reason, that works with
`xfdf` form data, but not with `fdf`. But then, of course, PDFtk isn't able to read that from stdin (unlike `fdf`,
of course), so it needs to be written to a temporary file for it.

Some aspects of this code were inspired by [pdffiller-stream], but all the code was written from scratch for this.


[ISC license]: https://en.wikipedia.org/wiki/ISC_license
[pdffiller-stream]: https://www.npmjs.com/package/pdffiller-stream
[PDFtk]: https://www.pdflabs.com/tools/pdftk-the-pdf-toolkit/
[Promise]: https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Promise
[Readable sream]: https://nodejs.org/api/stream.html#stream_class_stream_readable
