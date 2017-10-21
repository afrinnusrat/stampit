<img src="https://raw.githubusercontent.com/stampit-org/stampit-logo/master/stampit-logo.png" alt="stampit" width="160" />

# Stampit [![Build Status](https://travis-ci.org/stampit-org/stampit.svg?branch=master)](https://travis-ci.org/stampit-org/stampit) ![Greenkeeper Badge](https://badges.greenkeeper.io/stampit-org/stampit.svg) [![npm](https://img.shields.io/npm/dm/stampit.svg)](https://www.npmjs.com/package/stampit) [![Gitter](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/stampit-org/stampit?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge) [![Twitter Follow](https://img.shields.io/twitter/follow/stampit.svg?style=social&label=Follow&maxAge=2592000)](https://twitter.com/stampit_org)

**Create objects from reusable, composable behaviors** 
 
Stamps are [standardised](https://github.com/stampit-org/stamp-specification) composable factory functions. **Stampit** is an [infected compose](https://medium.com/@koresar/fun-with-stamps-episode-8-tracking-and-overriding-composition-573aa85ba622) featuring friendly handy API.

Stampit uses [three different kinds of prototypal OO](https://vimeo.com/69255635) to let you inherit behavior in a way that is much more powerful and flexible than classical OO.


## Example

```js
import stampit from 'stampit'

const Character = stampit({
  props: {
    name: null,
    health: 100
  },
  init({ name = this.name }) {
    this.name = name
  }
})

const Fighter = stampit(Character, {
  props: {
    stamina: 100
  },
  methods: {
    fight() {
      console.log(`${this.name} takes a mighty swing!`)
      this.stamina--
    }
  }
})

const Mage = stampit(Character, {
  props: {
    mana: 100
  },
  methods: {
    cast() {
      console.log(`${this.name} casts a fireball!`)
      this.mana--
    }
  }
})

const Paladin = stampit(Mage, Fighter) // as simple as that!

const fighter = Fighter({ name: 'Thumper' })
const mage = Mage({ name: 'Zapper' })
const paladin = Paladin({ name: 'Roland' })
```


## Status

* **v1**. `npm i stampit@1`
* **v2**. `npm i stampit@2` [Breaking changes](https://github.com/stampit-org/stampit/releases/tag/2.0)
* **v3**. `npm i stampit@3` [Breaking changes](https://github.com/stampit-org/stampit/releases/tag/v3.0.0). Compatible with the [stamp specification](https://github.com/stampit-org/stamp-specification) <= 1.4
* **v4**. `npm i stampit` [Breaking changes](https://github.com/stampit-org/stampit/releases/tag/v4.0.0). Compatible with the [stamp specification](https://github.com/stampit-org/stamp-specification) v1.5
* **next**. `npm i @stamp/it` The [new ecosystem](https://www.npmjs.com/~stamp/) of useful stamps like collision control, etc.


## Install

Via NPM:
[![NPM](https://nodei.co/npm/stampit.png?downloadRank=true)](https://www.npmjs.com/package/stampit)

Via bower:
```sh
$ bower install stampit
or
$ bower install stampit=https://npmcdn.com/stampit@4/dist/stampit.min.js
or
$ bower install stampit=https://unpkg.com/stampit@4.0.0/src/stampit.js
```

Browsers: [![CDNJS](https://img.shields.io/cdnjs/v/stampit.svg)](https://cdnjs.com/libraries/stampit)
[![UNPKG](https://img.shields.io/badge/unpkg.com--green.svg)](https://unpkg.com/stampit@latest/dist/stampit.umd.min.js)

## Compatibility

Stampit should run fine in any ES5 browser or any node.js.

## API

See the [API](docs/API.md).


# What's the Point?

Prototypal OO is great, and JavaScript's capabilities give us some really powerful tools to explore it, but it could be easier to use.

Basic questions like "how do I inherit privileged methods and private data?" and 
"what are some good alternatives to inheritance hierarchies?" are stumpers for many JavaScript users.

Let's answer both of these questions at the same time.

```js
// Some privileged methods with some private data.
const Availability = stampit().init(function() {
  var isOpen = false; // private

  this.open = function open() {
    isOpen = true;
    return this;
  };
  this.close = function close() {
    isOpen = false;
    return this;
  };
  this.isOpen = function isOpenMethod() {
    return isOpen;
  }
});

// Here's a stamp with public methods, and some state:
const Membership = stampit({
  props: {
    members: {}
  },
  methods: {
    add(member) {
      this.members[member.name] = member;
      return this;
    },
    getMember(name) {
      return this.members[name];
    }
  }
});

// Let's set some defaults:
const Defaults = stampit({
  props: {
    name: 'The Saloon',
    specials: 'Whisky, Gin, Tequila'
  },
  init({name, specials}) {
    this.name = name || this.name;
    this.specials = specials || this.specials;
  }
});

// Classical inheritance has nothing on this.
// No parent/child coupling. No deep inheritance hierarchies.
// Just good, clean code reusability.
const Bar = stampit(Defaults, Availability, Membership);

// Create an object instance
const myBar = Bar({name: 'Moe\'s'});

// Silly, but proves that everything is as it should be.
myBar.add({name: 'Homer'}).open().getMember('Homer');
```

For more examples see the [API](docs/API.md).


# More info

Stampit was written as an example for the book, ["Programming JavaScript Applications" (O'Reilly)](http://pjabook.com). See [this page](https://github.com/stampit-org/stampit/blob/master/docs/pjabook-updated-examples.md) to make book examples work with the latest stampit.

Looking for a deep dive into prototypal OO, stamps, and the Two Pillars of JavaScript? [Learn JavaScript with Eric Elliott](https://ericelliottjs.com).

**React Users.** Stampit *loves* React. Check out [react-stamp](https://github.com/stampit-org/react-stamp) for composable components.


# Development

### Unit tests
```
npm t
```

### Unit and benchmark tests
```
env CI=1 npm t
```

### Unit tests in a browser
To run unit tests in a default browser:
```
npm run browsertest
```
To run tests in a different browser:
* Open the `./test/index.html` in your browser, and
* open developer's console. The logs should indicate success.
