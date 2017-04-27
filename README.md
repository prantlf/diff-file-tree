# diff-file-tree

Compare two file-trees, get a list of changes, then apply left or right.

```
npm install diff-file-tree
```

## Usage

```js
var dft = require('diff-file-tree')

var changes = await dft.diff('/home/alice/stuff', '/home/alice/things')
console.log(changes) /* => [
  {change: 'mod', type: 'file', path: '/hello.txt'},
  {change: 'add', type: 'dir',  path: '/pics'},
  {change: 'add', type: 'file', path: '/pics/kitty.png'},
  {change: 'del', type: 'file', path: '/backup/hello.txt'},
  {change: 'del', type: 'dir',  path: '/backup'},
  {change: 'del', type: 'file', path: '/hello.txt'},
]*/

// make 'things' a copy of 'stuff' 
await dft.applyRight('/home/alice/stuff', '/home/alice/things', changes)
// -or-
// make 'stuff' a copy of 'things'
await dft.applyLeft('/home/alice/stuff', '/home/alice/things', changes)
```

## API

#### `var changes = await dft.diff(left, right[, opts])`

Get the differences between `left` and `right`.

Options include:

```js
{
  filter: null // optional function to ignore files (function (path) => bool)
  compareContent: false // set to true to compare by content instead of mtime & size
}
```

If you are using a custom fs module (like [graceful-fs](https://github.com/isaacs/node-graceful-fs) or [hyperdrive](https://github.com/mafintosh/hyperdrive)) you can pass that in with the left or right like this:

```js
dft.diff({path: '/Users/alice/stuff', fs: customFs}, {path: '/', fs: hyperdriveArchive})
```

#### `await dft.applyRight(left, right, changes)`

Make `right` equivalent to `left` using the given `changes`. Both `left` and `right` can be an object with custom `{path:, fs:}` as with `diff()`.

#### `await dft.applyLeft(left, right, changes)`

Make `left` equivalent to `right` using the given `changes`. Both `left` and `right` can be an object with custom `{path:, fs:}` as with `diff()`.