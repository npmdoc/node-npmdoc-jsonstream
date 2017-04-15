# api documentation for  [jsonstream (v1.0.3)](http://github.com/dominictarr/JSONStream)  [![npm package](https://img.shields.io/npm/v/npmdoc-jsonstream.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-jsonstream) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-jsonstream.svg)](https://travis-ci.org/npmdoc/node-npmdoc-jsonstream)
#### rawStream.pipe(JSONStream.parse()).pipe(streamOfObjects)

[![NPM](https://nodei.co/npm/jsonstream.png?downloads=true&downloadRank=true&stars=true)](https://www.npmjs.com/package/jsonstream)

[![apidoc](https://npmdoc.github.io/node-npmdoc-jsonstream/build/screenCapture.buildCi.browser.apidoc.html.png)](https://npmdoc.github.io/node-npmdoc-jsonstream/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-jsonstream/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-jsonstream/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "Dominic Tarr",
        "url": "http://bit.ly/dominictarr"
    },
    "bin": {
        "jsonstream": "./index.js"
    },
    "bugs": {
        "url": "https://github.com/dominictarr/JSONStream/issues"
    },
    "dependencies": {
        "jsonparse": "~1.0.0",
        "through": ">=2.2.7 <3"
    },
    "deprecated": "use JSONStream instead",
    "description": "rawStream.pipe(JSONStream.parse()).pipe(streamOfObjects)",
    "devDependencies": {
        "assertions": "~2.2.2",
        "event-stream": "~0.7.0",
        "it-is": "~1",
        "render": "~0.1.1",
        "tape": "~2.12.3",
        "trees": "~0.0.3"
    },
    "directories": {},
    "dist": {
        "shasum": "ff2d49c4f479b5bbcdf9f9e56c841cf87f0efa1d",
        "tarball": "https://registry.npmjs.org/jsonstream/-/jsonstream-1.0.3.tgz"
    },
    "engines": {
        "node": "*"
    },
    "gitHead": "ef6a0faed2340a38dbe961d460c863426dde1966",
    "homepage": "http://github.com/dominictarr/JSONStream",
    "keywords": [
        "json",
        "stream",
        "streaming",
        "parser",
        "async",
        "parsing"
    ],
    "maintainers": [
        {
            "name": "dominictarr"
        }
    ],
    "name": "jsonstream",
    "optionalDependencies": {},
    "repository": {
        "type": "git",
        "url": "git://github.com/dominictarr/JSONStream.git"
    },
    "scripts": {
        "test": "set -e; for t in test/*.js; do echo '***' $t '***'; node $t; done"
    },
    "version": "1.0.3"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module jsonstream](#apidoc.module.jsonstream)
1.  [function <span class="apidocSignatureSpan">jsonstream.</span>parse (path, map)](#apidoc.element.jsonstream.parse)
1.  [function <span class="apidocSignatureSpan">jsonstream.</span>stringify (op, sep, cl, indent)](#apidoc.element.jsonstream.stringify)
1.  [function <span class="apidocSignatureSpan">jsonstream.</span>stringifyObject (op, sep, cl, indent)](#apidoc.element.jsonstream.stringifyObject)



# <a name="apidoc.module.jsonstream"></a>[module jsonstream](#apidoc.module.jsonstream)

#### <a name="apidoc.element.jsonstream.parse"></a>[function <span class="apidocSignatureSpan">jsonstream.</span>parse (path, map)](#apidoc.element.jsonstream.parse)
- description and source-code
```javascript
parse = function (path, map) {

  var parser = new Parser()
  var stream = through(function (chunk) {
    if('string' === typeof chunk)
      chunk = new Buffer(chunk)
    parser.write(chunk)
  },
  function (data) {
    if(data)
      stream.write(data)
    stream.queue(null)
  })

  if('string' === typeof path)
    path = path.split('.').map(function (e) {
      if (e === '*')
        return true
      else if (e === '') // '..'.split('.') returns an empty string
        return {recurse: true}
      else
        return e
    })


  var count = 0, _key
  if(!path || !path.length)
    path = null

  parser.onValue = function (value) {
    if (!this.root)
      stream.root = value

    if(! path) return

    var i = 0 // iterates on path
    var j  = 0 // iterates on stack
    while (i < path.length) {
      var key = path[i]
      var c
      j++

      if (key && !key.recurse) {
        c = (j === this.stack.length) ? this : this.stack[j]
        if (!c) return
        if (! check(key, c.key)) return
        i++
      } else {
        i++
        var nextKey = path[i]
        if (! nextKey) return
        while (true) {
          c = (j === this.stack.length) ? this : this.stack[j]
          if (!c) return
          if (check(nextKey, c.key)) {
            i++;
            this.stack[j].value = null
            break
          }
          j++
        }
      }

    }
    if (j !== this.stack.length) return

    count ++
    var actualPath = this.stack.slice(1).map(function(element) { return element.key }).concat([this.key])
    var data = this.value[this.key]
    if(null != data)
      if(null != (data = map ? map(data, actualPath) : data))
        stream.queue(data)
    delete this.value[this.key]
    for(var k in this.stack)
      this.stack[k].value = null
  }
  parser._onToken = parser.onToken;

  parser.onToken = function (token, value) {
    parser._onToken(token, value);
    if (this.stack.length === 0) {
      if (stream.root) {
        if(!path)
          stream.queue(stream.root)
        count = 0;
        stream.root = null;
      }
    }
  }

  parser.onError = function (err) {
    if(err.message.indexOf("at position") > -1)
      err.message = "Invalid JSON (" + err.message + ")";
    stream.emit('error', err)
  }


  return stream
}
```
- example usage
```shell
...
  })

  return stream
}

if(!module.parent && process.title !== 'browser') {
  process.stdin
    .pipe(exports.parse(process.argv[2]))
    .pipe(exports.stringify('[', ',\n', ']\n', 2))
    .pipe(process.stdout)
}
...
```

#### <a name="apidoc.element.jsonstream.stringify"></a>[function <span class="apidocSignatureSpan">jsonstream.</span>stringify (op, sep, cl, indent)](#apidoc.element.jsonstream.stringify)
- description and source-code
```javascript
stringify = function (op, sep, cl, indent) {
  indent = indent || 0
  if (op === false){
    op = ''
    sep = '\n'
    cl = ''
  } else if (op == null) {

    op = '[\n'
    sep = '\n,\n'
    cl = '\n]\n'

  }

  //else, what ever you like

  var stream
    , first = true
    , anyData = false
  stream = through(function (data) {
    anyData = true
    var json = JSON.stringify(data, null, indent)
    if(first) { first = false ; stream.queue(op + json)}
    else stream.queue(sep + json)
  },
  function (data) {
    if(!anyData)
      stream.queue(op)
    stream.queue(cl)
    stream.queue(null)
  })

  return stream
}
```
- example usage
```shell
...
//else, what ever you like

var stream
  , first = true
  , anyData = false
stream = through(function (data) {
  anyData = true
  var json = JSON.stringify(data, null, indent)
  if(first) { first = false ; stream.queue(op + json)}
  else stream.queue(sep + json)
},
function (data) {
  if(!anyData)
    stream.queue(op)
  stream.queue(cl)
...
```

#### <a name="apidoc.element.jsonstream.stringifyObject"></a>[function <span class="apidocSignatureSpan">jsonstream.</span>stringifyObject (op, sep, cl, indent)](#apidoc.element.jsonstream.stringifyObject)
- description and source-code
```javascript
stringifyObject = function (op, sep, cl, indent) {
  indent = indent || 0
  if (op === false){
    op = ''
    sep = '\n'
    cl = ''
  } else if (op == null) {

    op = '{\n'
    sep = '\n,\n'
    cl = '\n}\n'

  }

  //else, what ever you like

  var first = true
    , anyData = false
  stream = through(function (data) {
    anyData = true
    var json = JSON.stringify(data[0]) + ':' + JSON.stringify(data[1], null, indent)
    if(first) { first = false ; this.queue(op + json)}
    else this.queue(sep + json)
  },
  function (data) {
    if(!anyData) this.queue(op)
    this.queue(cl)

    this.queue(null)
  })

  return stream
}
```
- example usage
```shell
...
the elements will only be seperated by a newline.

If you only write one item this will be valid JSON.

If you write many items,
you can use a 'RegExp' to split it into valid chunks.

## JSONStream.stringifyObject(open, sep, close)

Very much like 'JSONStream.stringify',
but creates a writable stream for objects instead of arrays.

Accordingly, 'open='{\n', sep='\n,\n', close='\n}\n''.

When you '.write()' to the stream you must supply an array with '[ key, data ]'
...
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
