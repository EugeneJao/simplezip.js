# SimpleZIP.js
A light-weight library for building uncompressed ZIP files, in NodeJS or in the Browser. Supported file name encoded by utf-8.

# Getting Started
## With NPM / NodeJS
```bash
npm install --save simplezip.js
```
And then
```js
const SimpleZip = require("simplezip.js");
const fs = require("fs");

let files = [{
    name: "FirstFile.txt",
    data: "The contents"
}, {
    name: "SecondFile.txt",
    data: Buffer.from("Also, the contents",'utf8')
}];

// This returns an ArrayBuffer
let data = SimpleZip.GenerateZipFrom(files);

fs.writeFileSync("output.zip", new Buffer(data), {encoding: 'binary'});

```

## With HTML / Javascript
It's easy! Add [simplezip.min.js](https://raw.githubusercontent.com/EugeneJao/simplezip.js/master/build/simplezip.min.js) to your project, then add
```html
<script type="text/javascript" src="?...../simplezip.min.js"></script>
```
to your headers, and then:
```js
var files = [{
    name: "FirstFile.txt",
    data: "The contents"
}, {
    name: "SecondFile.txt",
    data: new Uint8Array([65, 66, 67, 68])
}];

var data = SimpleZip.GenerateZipFrom(files);
// Can then convert to a blob and download the file
var blob = new Blob([data], {type: "octet/stream"});
var url = window.URL.createObjectURL(blob);

// Append to document, or whatever you want to do
var el = document.createElement("a");
el.href = url;
el.target = '_blank';
el.download = "output.zip";
el.innerHTML = "Download!"
document.getElementsByTagName("body")[0].appendChild(el);
``` 

# API
- [SimpleZIP.js](#simplezipjs)
- [Getting Started](#getting-started)
  - [With NPM / NodeJS](#with-npm--nodejs)
  - [With HTML / Javascript](#with-html--javascript)
- [API](#api)
    - [SimpleZip.GenerateZipFrom([files])](#simplezipgeneratezipfromfiles)
    - [new SimpleZip(expectedSize)](#new-simplezipexpectedsize)
    - [SimpleZip#appendFiles([files])](#simplezipappendfilesfiles)
    - [SimpleZip#appendFile(file)](#simplezipappendfilefile)
    - [SimpleZip#generate()](#simplezipgenerate)
    - [SimpleZip#ensureRemainingSpace(remaining)](#simplezipensureremainingspaceremaining)


Note that all 'files' passed to the SimpleZip API should be of the form
```ts
{
    name:string,
    data:ArrayBuffer|Uint8Array|string
}
```
---
<a name="generatezipfrom"></a>
### SimpleZip.GenerateZipFrom([files])
A static method to create the entire ZIP file all at once. This allocates the exact amount of space required, and fills it with the files specified.

---
<a name="constructor"></a>
### new SimpleZip(expectedSize)
Creates a new SimpleZip object, and allocates `expectedSize` bytes for the entire ZIP file. If this isn't enough, it will re-allocate later when adding new files.

---
<a name="appendfiles"></a>
### SimpleZip#appendFiles([files])
Appends the specified files to the ZIP file. Will re-allocate space to fit them all if required.

---
<a name="appendfile"></a>
### SimpleZip#appendFile(file)
Just a helper function. Calls [appendFiles](#appendfiles) with the file wrapped in an array.

---
<a name="generate"></a>
### SimpleZip#generate()
Appends the final ZIP records, and returns the ZIP file in an [ArrayBuffer](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/ArrayBuffer).
Will re-allocate space to fit the extra records if required. 

---
<a name="ensureremainingspace"></a>
### SimpleZip#ensureRemainingSpace(remaining)
Expands the capacity of the underlying ArrayBuffer to ensure at least `remaining` bytes are free to be filled. This also includes ZIP entry records and headers etc. So if you're resizing it to fit a file with a certain size, then add about 100 bytes more per file for the file names and ZIP entries etc.

---


