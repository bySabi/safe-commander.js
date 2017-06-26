# safe Commander.js

[![npm version](https://badge.fury.io/js/safe-commander.svg)](https://badge.fury.io/js/safe-commander)
[![npm downloads](https://img.shields.io/npm/dm/safe-commander.svg?style=flat-square)](https://www.npmjs.com/package/safe-commander)

  [Commander.js](https://github.com/tj/commander.js/) have a major design flaw in `option` API, adding options like: `name`, `opts`, `command`, `option` collide with commander instance. See issues: [#404](https://github.com/tj/commander.js/issues/404), [#584](https://github.com/tj/commander.js/issues/584), [#648](https://github.com/tj/commander.js/issues/648)

> `safe-commander` solves collision problem but with **breaking changes**.


## Installation
    $ npm install safe-commander --save

## Usage
Follow Commander [API documentation](http://tj.github.com/commander.js/)

## Breaking changes
Created options with `option` API will not be anymore on Commander instance object, a new object store `optsObj` was added.

original `Commander.js` Example:
```js
#!/usr/bin/env node

/**
 * Module dependencies.
 */

var program = require('commander');

program
  .version('0.1.0')
  .option('-p, --peppers', 'Add peppers')
  .option('-P, --pineapple', 'Add pineapple')
  .option('-b, --bbq-sauce', 'Add bbq sauce')
  .option('-c, --cheese [type]', 'Add the specified type of cheese [marble]', 'marble')
  .parse(process.argv);

console.log('you ordered a pizza with:');
if (program.peppers) console.log('  - peppers');
if (program.pineapple) console.log('  - pineapple');
if (program.bbqSauce) console.log('  - bbq');
console.log('  - %s cheese', program.cheese);
```

become to, with `safe-commander`:
```js
#!/usr/bin/env node

/**
 * Module dependencies.
 */

var program = require('safe-commander');   // safe-commander

program
  .version('0.1.0')
  .option('-p, --peppers', 'Add peppers')
  .option('-P, --pineapple', 'Add pineapple')
  .option('-b, --bbq-sauce', 'Add bbq sauce')
  .option('-c, --cheese [type]', 'Add the specified type of cheese [marble]', 'marble')
  .parse(process.argv);

console.log('you ordered a pizza with:');
if (program.optsObj.peppers) console.log('  - peppers');    // Breaking change!
if (program.optsObj.pineapple) console.log('  - pineapple');  // Breaking change!
if (program.optsObj.bbqSauce) console.log('  - bbq');         // Breaking change!
console.log('  - %s cheese', program.optsObj.cheese);         // Breaking change!
```

## Contributing

* Documentation improvement
* Feel free to send any PR

## License

[MIT][mit-license]

[mit-license]:./LICENSE
