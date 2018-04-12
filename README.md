[![License](https://img.shields.io/badge/license-MIT-blue.svg)](./LICENSE)

# Theatre.machine

> *A [finite-state machine (FSM)](https://en.wikipedia.org/wiki/Finite-state_machine) is a mathematical model of computation. It is an abstract machine that can be in exactly one of a finite number of states at any given time. The FSM can change from one state to another in response to some external inputs.*

## Overview

This Machine module helps you to create a Finite State Machine.

## Installation

Copy the content of [`sources/`](./sources) folder into your project.

Import the Machine module in order to use it in your application :

```javascript
import {Machine} from './my-path/machine.js';
```

## Usage

To use this Machine module, you'll need to instanciate it with your presets ([`map`](#constructor) object).

#### Quick start

We first start an instance of this Machine module :

```javascript
// starts a Machine instance with your presets
const runner = new Machine({

    'RUNNING': {

        'PRESSED_SPACE': 'JUMPING',
        'PRESSED_DOWN': 'SLIDING'
    },
    'JUMPING': {

        'GROUNDED': 'RUNNING'
    },
    'SLIDING': {

        'RELEASED_DOWN': 'RUNNING',
        'PRESSED_SPACE': 'JUMPING'
    }
});
```

Then, we initialize this new created machine specifying its initial state :

```javascript
runner.initialize('RUNNING');
```

We can retrieve the current state of the machine :

```javascript
// gets 'RUNNING'
console.log(runner.state());
```

We can pass this machine a message we need it to handle :

```javascript
// 'PRESSED_DOWN' is mapped onto current 'RUNNING' state and redirects to 'SLIDING' state
const target = runner.handle('PRESSED_DOWN');

// both gets 'SLIDING'
console.log(target);
console.log(runner.state());
```

If we pass this machine a message to handle which is not mapped to current state, nothing happens :

```javascript
// 'PRESSED_DOWN' is not mapped onto current 'SLIDING' state and doesn't redirect to any state
const attempt = runner.handle('PRESSED_DOWN');

// gets null
console.log(attempt);

// gets 'SLIDING'
console.log(runner.state());

// 'RELEASED_DOWN' is mapped onto current 'SLIDING' state and redirects to 'RUNNING' state
const target = runner.handle('RELEASED_DOWN');

// both gets 'RUNNING'
console.log(target);
console.log(runner.state());
```

## API

[`constructor()`](#constructor)

[`handle()`](#handle)

[`initialize()`](#initialize)

[`state()`](#state)

---

#### `constructor()`

Creates an instance of this Machine module.

###### Usage :

```javascript
// starts a Machine instance with your presets
const machine = new Machine(map);
```

###### Properties :

| property  | name  | type     | description                         |
| --------- | ----- | -------- | ----------------------------------- |
| parameter | `map` | `object` | defines machine's states and inputs |

###### Details :

`map` :

The `map` object purpose is to define the Finite State Machine cycle and its inputs following the example below.

```javascript
const map = {

    // state => handlers
    'STATE_A': {

        // input => target
        'INPUT_B': 'STATE_B',
        [...]
    },
    // state => handlers
    'STATE_B': {

        // input => target
        'INPUT_A': 'STATE_A',
        [...]
    }
}
```

---

#### `handle()`

Allows the machine to handle given input and to redirect to the mapped state.

###### Usage :

```javascript
// if current state can handle given input then update the machine and return the new state else nothing happens
const target = machine.handle(message);
```

###### Properties :

| property  | name      | type               | description            |
| --------- | --------- | ------------------ | ---------------------- |
| parameter | `message` | `string`           | message to handle      |
| return    | `target`  | `string` or `null` | mapped state or `null` |

---

#### `initialize()`

Starts the machine onto a specific existing state.

###### Usage :

```javascript
machine.initialize('STATE_A');
```

###### Properties :

| property  | name    | type     | description         |
| --------- | ------- | -------- | ------------------- |
| parameter | `state` | `string` | state to initialize |

---

#### `state()`

Gets the machine's current state.

###### Usage :

```javascript
// gets the machine's current state
const state = machine.state();
```

###### Properties :

| property | name    | type     | description   |
| -------- | ------- | -------- | ------------- |
| return   | `state` | `string` | current state |
