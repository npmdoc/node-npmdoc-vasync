# api documentation for  [vasync (v1.6.4)](https://github.com/davepacheco/node-vasync#readme)  [![npm package](https://img.shields.io/npm/v/npmdoc-vasync.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-vasync) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-vasync.svg)](https://travis-ci.org/npmdoc/node-npmdoc-vasync)
#### utilities for observable asynchronous control flow

[![NPM](https://nodei.co/npm/vasync.png?downloads=true&downloadRank=true&stars=true)](https://www.npmjs.com/package/vasync)

[![apidoc](https://npmdoc.github.io/node-npmdoc-vasync/build/screenCapture.buildCi.browser.%252Ftmp%252Fbuild%252Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-vasync/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-vasync/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-vasync/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "bugs": {
        "url": "https://github.com/davepacheco/node-vasync/issues"
    },
    "dependencies": {
        "verror": "1.6.0"
    },
    "description": "utilities for observable asynchronous control flow",
    "devDependencies": {
        "nodeunit": "0.8.7",
        "tap": "~0.4.8"
    },
    "directories": {},
    "dist": {
        "shasum": "dfe93616ad0e7ae801b332a9d88bfc5cdc8e1d1f",
        "tarball": "https://registry.npmjs.org/vasync/-/vasync-1.6.4.tgz"
    },
    "engines": [
        "node >=0.6.0"
    ],
    "gitHead": "e7a557e09fea5fcd712324e24f8859c8f1f12736",
    "homepage": "https://github.com/davepacheco/node-vasync#readme",
    "license": "MIT",
    "main": "./lib/vasync.js",
    "maintainers": [
        {
            "name": "dap"
        }
    ],
    "name": "vasync",
    "optionalDependencies": {},
    "repository": {
        "type": "git",
        "url": "git://github.com/davepacheco/node-vasync.git"
    },
    "scripts": {
        "test": "tap tests/ && ./node_modules/.bin/nodeunit tests/compat.js"
    },
    "version": "1.6.4"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module vasync](#apidoc.module.vasync)
1.  [function <span class="apidocSignatureSpan">vasync.</span>barrier (args)](#apidoc.element.vasync.barrier)
1.  [function <span class="apidocSignatureSpan">vasync.</span>forEachParallel (args, callback)](#apidoc.element.vasync.forEachParallel)
1.  [function <span class="apidocSignatureSpan">vasync.</span>forEachPipeline (args, callback)](#apidoc.element.vasync.forEachPipeline)
1.  [function <span class="apidocSignatureSpan">vasync.</span>parallel (args, callback)](#apidoc.element.vasync.parallel)
1.  [function <span class="apidocSignatureSpan">vasync.</span>pipeline (args, callback)](#apidoc.element.vasync.pipeline)
1.  [function <span class="apidocSignatureSpan">vasync.</span>queue (worker, concurrency)](#apidoc.element.vasync.queue)
1.  [function <span class="apidocSignatureSpan">vasync.</span>queuev (args)](#apidoc.element.vasync.queuev)
1.  [function <span class="apidocSignatureSpan">vasync.</span>waterfall (funcs, callback)](#apidoc.element.vasync.waterfall)



# <a name="apidoc.module.vasync"></a>[module vasync](#apidoc.module.vasync)

#### <a name="apidoc.element.vasync.barrier"></a>[function <span class="apidocSignatureSpan">vasync.</span>barrier (args)](#apidoc.element.vasync.barrier)
- description and source-code
```javascript
function barrier(args)
{
	return (new Barrier(args));
}
```
- example usage
```shell
...
Example: printing sizes of files in a directory

'''js
var mod_fs = require('fs');
var mod_path = require('path');
var mod_vasync = require('../lib/vasync');

var barrier = mod_vasync.barrier();

barrier.on('drain', function () {
  console.log('all files checked');
});

barrier.start('readdir');
...
```

#### <a name="apidoc.element.vasync.forEachParallel"></a>[function <span class="apidocSignatureSpan">vasync.</span>forEachParallel (args, callback)](#apidoc.element.vasync.forEachParallel)
- description and source-code
```javascript
function forEachParallel(args, callback)
{
	var func, funcs;

	mod_assert.equal(typeof (args), 'object', '"args" must be an object');
	mod_assert.equal(typeof (args['func']), 'function',
	    '"args.func" must be specified and must be a function');
	mod_assert.ok(Array.isArray(args['inputs']),
	    '"args.inputs" must be specified and must be an array');

	func = args['func'];
	funcs = args['inputs'].map(function (input) {
		return (function (subcallback) {
			return (func(input, subcallback));
		});
	});

	return (parallel({ 'funcs': funcs }, callback));
}
```
- example usage
```shell
...
This function is exactly like 'parallel', except that the input is specified as
a *single* function ("func") and a list of inputs ("inputs").  The function is
invoked on each input in parallel.

This example is exactly equivalent to the one above:

'''js
console.log(mod_vasync.forEachParallel({
    'func': mod_dns.resolve,
    'inputs': [ 'joyent.com', 'github.com', 'asdfaqsdfj.com' ]
}, function (err, results) {
    console.log('error: %s', err.message);
    console.log('results: %s', mod_util.inspect(results, null, 3));
}));
'''
...
```

#### <a name="apidoc.element.vasync.forEachPipeline"></a>[function <span class="apidocSignatureSpan">vasync.</span>forEachPipeline (args, callback)](#apidoc.element.vasync.forEachPipeline)
- description and source-code
```javascript
function forEachPipeline(args, callback) {
    mod_assert.equal(typeof (args), 'object', '"args" must be an object');
    mod_assert.equal(typeof (args['func']), 'function',
		'"args.func" must be specified and must be a function');
    mod_assert.ok(Array.isArray(args['inputs']),
		'"args.inputs" must be specified and must be an array');
    mod_assert.equal(typeof (callback), 'function',
		'callback argument must be specified and must be a function');

    var func = args['func'];

    var funcs = args['inputs'].map(function (input) {
		return (function (_, subcallback) {
			return (func(input, subcallback));
		});
    });

    return (pipeline({'funcs': funcs}, callback));
}
```
- example usage
```shell
...
This function is exactly like 'pipeline', except that the input is specified as
a *single* function ("func") and a list of inputs ("inputs").  The function is
invoked on each input in series.

This example is exactly equivalent to the one above:

'''js
console.log(mod_vasync.forEachPipeline({
    'func': mod_dns.resolve,
    'inputs': [ 'joyent.com', 'github.com', 'asdfaqsdfj.com' ]
}, function (err, results) {
    console.log('error: %s', err.message);
    console.log('results: %s', mod_util.inspect(results, null, 3));
}));
'''
...
```

#### <a name="apidoc.element.vasync.parallel"></a>[function <span class="apidocSignatureSpan">vasync.</span>parallel (args, callback)](#apidoc.element.vasync.parallel)
- description and source-code
```javascript
function parallel(args, callback)
{
	var funcs, rv, doneOne, i;

	mod_assert.equal(typeof (args), 'object', '"args" must be an object');
	mod_assert.ok(Array.isArray(args['funcs']),
	    '"args.funcs" must be specified and must be an array');
	mod_assert.equal(typeof (callback), 'function',
	    'callback argument must be specified and must be a function');

	funcs = args['funcs'].slice(0);

	rv = {
	    'operations': new Array(funcs.length),
	    'successes': [],
	    'ndone': 0,
	    'nerrors': 0
	};

	if (funcs.length === 0) {
		setImmediate(function () { callback(null, rv); });
		return (rv);
	}

	doneOne = function (entry) {
		return (function (err, result) {
			mod_assert.equal(entry['status'], 'pending');

			entry['err'] = err;
			entry['result'] = result;
			entry['status'] = err ? 'fail' : 'ok';

			if (err)
				rv['nerrors']++;
			else
				rv['successes'].push(result);

			if (++rv['ndone'] < funcs.length)
				return;

			var errors = rv['operations'].filter(function (ent) {
				return (ent['status'] == 'fail');
			}).map(function (ent) { return (ent['err']); });

			if (errors.length > 0)
				callback(new mod_verror.MultiError(errors), rv);
			else
				callback(null, rv);
		});
	};

	for (i = 0; i < funcs.length; i++) {
		rv['operations'][i] = {
			'func': funcs[i],
			'funcname': funcs[i].name || '(anon)',
			'status': 'pending'
		};

		funcs[i](doneOne(rv['operations'][i]));
	}

	return (rv);
}
```
- example usage
```shell
...

All errors are combined into a single "err" parameter to the final callback (see
below).

Example usage:

'''js
console.log(mod_vasync.parallel({
    'funcs': [
function f1 (callback) { mod_dns.resolve('joyent.com', callback); },
function f2 (callback) { mod_dns.resolve('github.com', callback); },
function f3 (callback) { mod_dns.resolve('asdfaqsdfj.com', callback); }
    ]
}, function (err, results) {
console.log('error: %s', err.message);
...
```

#### <a name="apidoc.element.vasync.pipeline"></a>[function <span class="apidocSignatureSpan">vasync.</span>pipeline (args, callback)](#apidoc.element.vasync.pipeline)
- description and source-code
```javascript
function pipeline(args, callback)
{
	var funcs, uarg, rv, next;

	mod_assert.equal(typeof (args), 'object', '"args" must be an object');
	mod_assert.ok(Array.isArray(args['funcs']),
	    '"args.funcs" must be specified and must be an array');

	funcs = args['funcs'].slice(0);
	uarg = args['arg'];

	rv = {
	    'operations': funcs.map(function (func) {
		return ({
		    'func': func,
		    'funcname': func.name || '(anon)',
		    'status': 'waiting'
		});
	    }),
	    'successes': [],
	    'ndone': 0,
	    'nerrors': 0
	};

	if (funcs.length === 0) {
		setImmediate(function () { callback(null, rv); });
		return (rv);
	}

	next = function (err, result) {
		if (rv['nerrors'] > 0 ||
		    rv['ndone'] >= rv['operations'].length) {
			throw new mod_verror.VError('pipeline callback ' +
			    'invoked after the pipeline has already ' +
			    'completed (%j)', rv);
		}

		var entry = rv['operations'][rv['ndone']++];

		mod_assert.equal(entry['status'], 'pending');

		entry['status'] = err ? 'fail' : 'ok';
		entry['err'] = err;
		entry['result'] = result;

		if (err)
			rv['nerrors']++;
		else
			rv['successes'].push(result);

		if (err || rv['ndone'] == funcs.length) {
			callback(err, rv);
		} else {
			var nextent = rv['operations'][rv['ndone']];
			nextent['status'] = 'pending';

			<span class="apidocCodeCommentSpan">/*
			 * We invoke the next function on the next tick so that
			 * the caller (stage N) need not worry about the case
			 * that the next stage (stage N + 1) runs in its own
			 * context.
			 */
</span>			setImmediate(function () {
				nextent['func'](uarg, next);
			});
		}
	};

	rv['operations'][0]['status'] = 'pending';
	funcs[0](uarg, next);

	return (rv);
}
```
- example usage
```shell
...
for 'parallel'.  The error object for the final callback is just the error
returned by whatever pipeline function failed (if any).

This example is similar to the one above, except that it runs the steps in
sequence and stops early because 'pipeline' stops on the first error:

'''js
console.log(mod_vasync.pipeline({
    'funcs': [
function f1 (_, callback) { mod_fs.stat('/tmp', callback); },
function f2 (_, callback) { mod_fs.stat('/noexist', callback); },
function f3 (_, callback) { mod_fs.stat('/var', callback); }
    ]
}, function (err, results) {
console.log('error: %s', err.message);
...
```

#### <a name="apidoc.element.vasync.queue"></a>[function <span class="apidocSignatureSpan">vasync.</span>queue (worker, concurrency)](#apidoc.element.vasync.queue)
- description and source-code
```javascript
function queue(worker, concurrency)
{
	return (new WorkQueue({
	    'worker': worker,
	    'concurrency': concurrency
	}));
}
```
- example usage
```shell
...

function doneOne()
{
  console.log('task completed; queue state:\n%s\n',
      JSON.stringify(queue, null, 4));
}

queue = mod_vasync.queue(mod_fs.stat, 2);

console.log('initial queue state:\n%s\n', JSON.stringify(queue, null, 4));

queue.push('/tmp/file1', doneOne);
queue.push('/tmp/file2', doneOne);
queue.push('/tmp/file3', doneOne);
queue.push('/tmp/file4', doneOne);
...
```

#### <a name="apidoc.element.vasync.queuev"></a>[function <span class="apidocSignatureSpan">vasync.</span>queuev (args)](#apidoc.element.vasync.queuev)
- description and source-code
```javascript
function queuev(args)
{
	return (new WorkQueue(args));
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.vasync.waterfall"></a>[function <span class="apidocSignatureSpan">vasync.</span>waterfall (funcs, callback)](#apidoc.element.vasync.waterfall)
- description and source-code
```javascript
function waterfall(funcs, callback)
{
	var rv, current, next;

	mod_assert.ok(Array.isArray(funcs));
	mod_assert.ok(arguments.length == 1 || typeof (callback) == 'function');
	funcs = funcs.slice(0);

	rv = {
	    'operations': funcs.map(function (func) {
	        return ({
		    'func': func,
		    'funcname': func.name || '(anon)',
		    'status': 'waiting'
		});
	    }),
	    'successes': [],
	    'ndone': 0,
	    'nerrors': 0
	};

	if (funcs.length === 0) {
		if (callback)
			setImmediate(function () { callback(null, rv); });
		return (rv);
	}

	next = function (idx, err) {
		var args, entry, nextentry;

		if (err === undefined)
			err = null;

		if (idx != current) {
			throw (new mod_verror.VError(
			    'vasync.waterfall: function %d ("%s") invoked ' +
			    'its callback twice', idx,
			    rv['operations'][idx].funcname));
		}

		mod_assert.equal(idx, rv['ndone']);
		entry = rv['operations'][rv['ndone']++];
		args = Array.prototype.slice.call(arguments, 2);

		mod_assert.equal(entry['status'], 'pending');
		entry['status'] = err ? 'fail' : 'ok';
		entry['err'] = err;
		entry['results'] = args;

		if (err)
			rv['nerrors']++;
		else
			rv['successes'].push(args);

		if (err || rv['ndone'] == funcs.length) {
			if (callback) {
				args.unshift(err);
				callback.apply(null, args);
			}
		} else {
			nextentry = rv['operations'][rv['ndone']];
			nextentry['status'] = 'pending';
			current++;
			args.push(next.bind(null, current));
			setImmediate(function () {
				nextentry['func'].apply(null, args);
			});
		}
	};

	rv['operations'][0]['status'] = 'pending';
	current = 0;
	funcs[0](next.bind(null, current));
	return (rv);
}
```
- example usage
```shell
...
to each function in the chain.  pipeline() always passes the same argument
followed by the callback, while waterfall() passes whatever values were emitted
by the previous function followed by the callback.

Here's an example:

'''js
mod_vasync.waterfall([
    function func1(callback) {
 	setImmediate(function () {
		callback(null, 37);
	});
    },
    function func2(extra, callback) {
	console.log('func2 got "%s" from func1', extra);
...
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
