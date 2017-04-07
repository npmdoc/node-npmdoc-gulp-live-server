# api documentation for  [gulp-live-server (v0.0.30)](https://github.com/gimm/gulp-live-server)  [![npm package](https://img.shields.io/npm/v/npmdoc-gulp-live-server.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-gulp-live-server) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-gulp-live-server.svg)](https://travis-ci.org/npmdoc/node-npmdoc-gulp-live-server)
#### easy light weight server with livereload

[![NPM](https://nodei.co/npm/gulp-live-server.png?downloads=true)](https://www.npmjs.com/package/gulp-live-server)

[![apidoc](https://npmdoc.github.io/node-npmdoc-gulp-live-server/build/screenCapture.buildNpmdoc.browser._2Fhome_2Ftravis_2Fbuild_2Fnpmdoc_2Fnode-npmdoc-gulp-live-server_2Ftmp_2Fbuild_2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-gulp-live-server/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-gulp-live-server/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-gulp-live-server/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "yucc2008@gmail.com"
    },
    "bugs": {
        "url": "https://github.com/gimm/gulp-live-server/issues"
    },
    "dependencies": {
        "chalk": "^1.0.0",
        "connect": "^3.3.4",
        "connect-livereload": "^0.5.3",
        "debug": "^2.1.1",
        "deepmerge": "~0.2.7",
        "event-stream": "~3.2.1",
        "q": "^1.2.0",
        "serve-static": "^1.9.1",
        "tiny-lr": "0.0.9"
    },
    "description": "easy light weight server with livereload",
    "devDependencies": {
        "gulp": "^3.8.11",
        "mocha": "^2.0.1",
        "should": "^5.2.0",
        "supertest": "^0.15.0"
    },
    "directories": {},
    "dist": {
        "shasum": "c4c3128bd1c3249aa0eb4f050c0f71bee87707de",
        "tarball": "https://registry.npmjs.org/gulp-live-server/-/gulp-live-server-0.0.30.tgz"
    },
    "gitHead": "b645c06f13729f0406692130cb529b21d60d5e15",
    "homepage": "https://github.com/gimm/gulp-live-server",
    "keywords": [
        "gulpplugin",
        "server",
        "static",
        "live",
        "livereload",
        "connect",
        "express"
    ],
    "license": "WTFPL",
    "main": "./index.js",
    "maintainers": [
        {
            "name": "yucc2008",
            "email": "yucc2008@gmail.com"
        }
    ],
    "name": "gulp-live-server",
    "optionalDependencies": {},
    "readme": "ERROR: No README data found!",
    "repository": {
        "type": "git",
        "url": "git://github.com/gimm/gulp-live-server.git"
    },
    "scripts": {
        "test": "mocha"
    },
    "version": "0.0.30"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module gulp-live-server](#apidoc.module.gulp-live-server)
1.  [function <span class="apidocSignatureSpan">gulp-live-server.</span>new (script)](#apidoc.element.gulp-live-server.new)
1.  [function <span class="apidocSignatureSpan">gulp-live-server.</span>notify (event)](#apidoc.element.gulp-live-server.notify)
1.  [function <span class="apidocSignatureSpan">gulp-live-server.</span>start (execPath)](#apidoc.element.gulp-live-server.start)
1.  [function <span class="apidocSignatureSpan">gulp-live-server.</span>static (folder, port)](#apidoc.element.gulp-live-server.static)
1.  [function <span class="apidocSignatureSpan">gulp-live-server.</span>stop ()](#apidoc.element.gulp-live-server.stop)
1.  string <span class="apidocSignatureSpan">gulp-live-server.</span>script



# <a name="apidoc.module.gulp-live-server"></a>[module gulp-live-server](#apidoc.module.gulp-live-server)

#### <a name="apidoc.element.gulp-live-server.new"></a>[function <span class="apidocSignatureSpan">gulp-live-server.</span>new (script)](#apidoc.element.gulp-live-server.new)
- description and source-code
```javascript
new = function (script) {
    if(!script){
        return console.log(error('script file not specified.'));
    }
    var args = util.isArray(script) ? script : [script];
    return this(args);
}
```
- example usage
```shell
...
  });
    '''
- Serve with your own script file

  '''js
    gulp.task('serve', function() {
//1. run your script as a server
var server = gls.new('myapp.js');
server.start();

//2. run script with cwd args, e.g. the harmony flag
var server = gls.new(['--harmony', 'myapp.js']);
//this will achieve 'node --harmony myapp.js'
//you can access cwd args in 'myapp.js' via 'process.argv'
server.start();
...
```

#### <a name="apidoc.element.gulp-live-server.notify"></a>[function <span class="apidocSignatureSpan">gulp-live-server.</span>notify (event)](#apidoc.element.gulp-live-server.notify)
- description and source-code
```javascript
notify = function (event) {
	var lr = this.lr;
    if(event && event.path){
        var filepath = path.relative(__dirname, event.path);
        debug(info('file(s) changed: %s'), event.path);
        lr.changed({body: {files: [filepath]}});
    }

    return es.map(function(file, done) {
        var filepath = path.relative(__dirname, file.path);
        debug(info('file(s) changed: %s'), filepath);
        lr.changed({body: {files: [filepath]}});
        done(null, file);
    }.bind(this));
}
```
- example usage
```shell
...
var deferred = Q.defer();
this.server = spawn(this.config.execPath, this.config.args, this.config.options);

//stdout and stderr will not be set if using the { stdio: 'inherit' } options for spawn
if (this.server.stdout) {
    this.server.stdout.setEncoding('utf8');
    this.server.stdout.on('data', function(data) {
        deferred.notify(data);
        callback.serverLog(data);
    });
}
if (this.server.stderr) {
    this.server.stderr.setEncoding('utf8');
    this.server.stderr.on('data', function (data) {
        deferred.notify(data);
...
```

#### <a name="apidoc.element.gulp-live-server.start"></a>[function <span class="apidocSignatureSpan">gulp-live-server.</span>start (execPath)](#apidoc.element.gulp-live-server.start)
- description and source-code
```javascript
start = function (execPath) {
    if (this.server) { // server already running
        debug(info('kill server'));
        this.server.kill('SIGKILL');
        //server.removeListener('exit', callback.serverExit);
        this.server = undefined;
    } else {
        if(this.config.livereload){
            this.lr = tinylr(this.config.livereload);
            this.lr.listen(this.config.livereload.port, callback.lrServerReady.bind(this));
        }
    }

    // if a executable is specified use that to start the server (e.g. coffeescript)
    // otherwise use the currents process executable
    this.config.execPath =  execPath || this.config.execPath || process.execPath;
    var deferred = Q.defer();
    this.server = spawn(this.config.execPath, this.config.args, this.config.options);

    //stdout and stderr will not be set if using the { stdio: 'inherit' } options for spawn
    if (this.server.stdout) {
        this.server.stdout.setEncoding('utf8');
        this.server.stdout.on('data', function(data) {
            deferred.notify(data);
            callback.serverLog(data);
        });
    }
    if (this.server.stderr) {
        this.server.stderr.setEncoding('utf8');
        this.server.stderr.on('data', function (data) {
            deferred.notify(data);
            callback.serverError(data);
        });
    }

    this.server.once('exit', function (code, sig) {
        setTimeout(function() { // yield event loop for stdout/stderr
          deferred.resolve({
              code: code,
              signal: sig
          });
          if (glsInstanceCounter == 0)
            callback.serverExit(code, sig);
        }, 0)
    });

    var exit = function(code, sig) {
      callback.processExit(code,sig,server);
    }
    process.listeners('exit') || process.once('exit', exit);

    glsInstanceCounter++;
    return deferred.promise;
}
```
- example usage
```shell
...

  '''js
var gulp = require('gulp');
var gls = require('gulp-live-server');
gulp.task('serve', function() {
//1. serve with default settings
var server = gls.static(); //equals to gls.static('public', 3000);
server.start();

//2. serve at custom port
var server = gls.static('dist', 8888);
server.start();

//3. serve multi folders
var server = gls.static(['dist', '.tmp']);
...
```

#### <a name="apidoc.element.gulp-live-server.static"></a>[function <span class="apidocSignatureSpan">gulp-live-server.</span>static (folder, port)](#apidoc.element.gulp-live-server.static)
- description and source-code
```javascript
static = function (folder, port) {
    var script = this.script;
    folder = folder || process.cwd();
    util.isArray(folder) && (folder = folder.join(','));
    port = port || 3000;
    return this([script, folder, port]);
}
```
- example usage
```shell
...
- Serve a static folder('gls.script'<'scripts/static.js'> is used as server script)

  '''js
var gulp = require('gulp');
var gls = require('gulp-live-server');
gulp.task('serve', function() {
//1. serve with default settings
var server = gls.static(); //equals to gls.static('public', 3000);
server.start();

//2. serve at custom port
var server = gls.static('dist', 8888);
server.start();

//3. serve multi folders
...
```

#### <a name="apidoc.element.gulp-live-server.stop"></a>[function <span class="apidocSignatureSpan">gulp-live-server.</span>stop ()](#apidoc.element.gulp-live-server.stop)
- description and source-code
```javascript
stop = function () {
    var deferred = Q.defer();
    if (this.server) {
        this.server.once('exit', function (code) {
            deferred.resolve(code);
        });

        debug(info('kill server'));
        //use SIGHUP instead of SIGKILL, see issue #34
        this.server.kill('SIGKILL');
        //server.removeListener('exit', callback.serverExit);
        this.server = undefined;
    }else{
        deferred.resolve(0);
    }
    if(this.lr){
        debug(info('close livereload server'));
        this.lr.close();
        //TODO how to stop tiny-lr from hanging the terminal
        this.lr = undefined;
    }

    return deferred.promise;
}
```
- example usage
```shell
n/a
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
