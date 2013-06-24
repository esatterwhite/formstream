formstream [![Build Status](https://secure.travis-ci.org/fengmk2/formstream.png)](http://travis-ci.org/fengmk2/formstream) [![Coverage Status](https://coveralls.io/repos/fengmk2/formstream/badge.png)](https://coveralls.io/r/fengmk2/formstream)
==========

![logo](https://raw.github.com/fengmk2/formstream/master/logo.png)

A [multipart/form-data](http://tools.ietf.org/html/rfc2388) encoded stream, helper for file upload.

## Install

```bash
$ npm install formstream
```

## Usage

```js
var formstream = require('formstream');
var http = require('http');

var form = formstream();

// form.file('file', filepath, filename);
form.file('file', './logo.png', 'upload-logo.png');

// other form fields
form.field('foo', 'fengmk2');
form.field('love', 'aerdeng');

// even send file content buffer directly
// form.buffer(name, buffer, filename, mimeType)
form.buffer('file2', new Buffer('This is file2 content.'), 'foo.txt');

var options = {
  method: 'POST',
  host: 'upload.cnodejs.net',
  path: '/store',
  headers: form.headers()
};
var req = http.request(options, function (res) {
  console.log('Status: %s', res.statusCode);
  res.on('data', function (data) {
    console.log(data.toString());
  });
});

form.pipe(req);
```

### `form.setTotalStreamSize(size)`: Upload file with `Content-Length`

If you know the `ReadStream` total size and you must to set `Content-Length`.
You may want to use `form.setTotalStreamSize(size)`.

```js
var formstream = require('formstream');
var http = require('http');
var fs = require('fs');

fs.stat('./logo.png', function (err, stat) {
  var form = formstream();
  form.file('file', './logo.png', 'upload-logo.png');
  form.setTotalStreamSize(stat.size);
  var options = {
    method: 'POST',
    host: 'upload.cnodejs.net',
    path: '/store',
    headers: form.headers()
  };
  var req = http.request(options, function (res) {
    console.log('Status: %s', res.statusCode);
    res.on('data', function (data) {
      console.log(data.toString());
    });
  });
  form.pipe(req);
});
```

## License 

(The MIT License)

Copyright (c) 2012 - 2013 fengmk2 &lt;fengmk2@gmail.com&gt;

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
