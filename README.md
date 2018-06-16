# npmdoc-jsonstream

#### basic api documentation for  [jsonstream (1.0.3)](http://github.com/dominictarr/JSONStream)  [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-jsonstream.svg)](https://travis-ci.org/npmdoc/node-npmdoc-jsonstream)

#### rawStream.pipe(JSONStream.parse()).pipe(streamOfObjects)

[![NPM](https://nodei.co/npm/jsonstream.png?downloads=true&downloadRank=true&stars=true)](https://www.npmjs.com/package/jsonstream)

- [https://npmdoc.github.io/node-npmdoc-jsonstream/build/apidoc.html](https://npmdoc.github.io/node-npmdoc-jsonstream/build/apidoc.html)

[![apidoc](https://npmdoc.github.io/node-npmdoc-jsonstream/build/screenshot.buildCi.browser.%252Ftmp%252Fbuild%252Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-jsonstream/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-jsonstream/build/screenshot.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-jsonstream/build/screenshot.npmPackageDependencyTree.svg)



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



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
