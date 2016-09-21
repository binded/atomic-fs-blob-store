# atomic-fs-blob-store

[blob store](https://github.com/maxogden/abstract-blob-store) that atomically stores blobs (e.g. no partial writes) on the local file system.

Forked from [fs-blob-store](https://github.com/maxogden/fs-blob-store)

```sh
npm install atomic-fs-blob-store
```

[![build status](http://img.shields.io/travis/blockai/atomic-fs-blob-store.svg?style=flat)](http://travis-ci.org/atomic/atomic-fs-blob-store)

[![blob-store-compatible](https://raw.githubusercontent.com/maxogden/abstract-blob-store/master/badge.png)](https://github.com/maxogden/abstract-blob-store)

## Usage

``` js
var fs = require('atomic-fs-blob-store')
var blobs = fs('some-directory')

var ws = blobs.createWriteStream({
  key: 'some/path/file.txt'
})

ws.write('hello world\n')
ws.end(function() {
  var rs = blobs.createReadStream({
    key: 'some/path/file.txt'
  })

  rs.pipe(process.stdout)
})
```

## Atomicity

The original `fs-blob-store` doesn't make atomic writes which may lead
to partially written files when an error occurs or if the process
crashes.

`atomic-fs-blob-store` guarantees write atomicity, which means that if
your process crashes in the middle of a write, the file won't be written
at all.

Which mean that a key only starts to exist and becomes available for
reading once a write is fully completed.

## License

MIT
