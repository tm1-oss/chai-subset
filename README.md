
"containSubset" object properties matcher for [Chai](http://chaijs.com/) assertion library

Difference from the original package: Better error messages and contained arrays must contain identical objects in order

Installation
===========

`npm install --save-dev @tm1-oss/chai-subset`

Usage
=====

common.js
```js
var chai = require('chai');
var chaiSubset = require('@tm1-oss/chai-subset');
chai.use(chaiSubset);
```

in your spec.js
```js
var obj = {
	a: 'b',
	c: 'd',
	e: {
		foo: 'bar',
		baz: {
			qux: 'quux'
		}
	},
	f: [ { g: 'h' }, { i: 'j' } ]
};
	
expect(obj).to.containSubset({
	a: 'b',
	e: {
		baz: {
			qux: 'quux'
		}
	}
});

// Assertion error IS NOT thrown
expect(obj).to.containSubset({
	f: [ { g: 'h' }, { i: 'j' } ]
});

// Assertion error IS thrown as the contained array is not identical to the array in obj.
expect(obj).to.containSubset({
	f: [ { i: 'j' } ]
});


// or using a compare function
expect(obj).containSubset({
	a: (expectedValue) => expectedValue,
	c: (expectedValue) => expectedValue === 'd'
})

// or with 'not'
expect(obj).to.not.containSubset({
	g: 'whatever'
});
```

Also works good with arrays and `should` interface
```js
var list = [{a: 'a', b: 'b'}, {v: 'f', d: {z: 'g'}}];

list.should.containSubset([{a:'a'}]); //Assertion error is thrown
list.should.containSubset([{a:'a',  b: 'b'}]); //Assertion error is thrown

list.should.containSubset([{a:'a', b: 'bd'}]); 
/*throws
AssertionError: expected
[
    {
        "a": "a",
        "b": "b"
    },
    {
        "v": "f",
        "d": {
            "z": "g"
        }
    }
]
to contain subset 
[ { a: 'a', b: 'bd' } ]
*/
```

and with `assert` interface
```js
assert.containSubset({a: 1, b: 2}, {a: 1});
```
