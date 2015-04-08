# nano-blue
**A (bluebird promisified) minimalistic couchdb driver for node.js**

`nano-blue` wraps [nano][] in [bluebird-flavored][] promises.

[nano]: http://github.com/dscape/nano
[bluebird-flavored]: http://github.com/petkaantonov/bluebird

## Example
A rewriting of the first example from the `nano` docs:

```javascript
var nano = require('nano-blue')('http://localhost:5984');

// specify the database we are going to use
var alice = nano.use('alice');

// clean up a database we created previously
nano.db.destroy('alice')
  .then(function() {
    // create a new database
    return nano.db.create('alice');
  }).then(function() {
    // insert a document into the database
    return alice.insert({ crazy: true }, 'rabbit');
  }).spread(function(body, header) {
    console.log('you have inserted the rabbit');
    console.log(body);
  }).catch(function(err) {
    console.log(err.message);
  });


// you have inserted the rabbit
// { ok: true,
//   id: 'rabbit',
//   rev: '1-6e4cb465d49c0368ac3946506d26335d' }
```

Where you would normally use a callback that accepts `body` and `header`
arguments, you should probably use Bluebird's `.spread` instead of `.then`.  If
you just use `.then`, the values will be passed as an array, which makes things
not as nice. For example:

```javascript
// using .spread()
alice.insert({ crazy: true }, 'rabbit')
  .spread(function(body, header) {
    console.log(body)
    console.log(header)
  });


// using .then()
alice.insert({ crazy: true }, 'rabbit')
  .then(function(res) {
    console.log(res[0])
    console.log(res[1])
  });
```

---

## License (MIT)

See `LICENSE` file for details.

`nano` itself is licensed under the [Apache License, version 2.0]

[Apache License, version 2.0]: http://www.apache.org/licenses/LICENSE-2.0.html
