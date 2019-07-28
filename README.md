# command-line-application

[![CircleCI](https://img.shields.io/circleci/project/github/hipstersmoothie/command-line-application/master.svg?style=for-the-badge)](https://circleci.com/gh/hipstersmoothie/command-line-application) [![npm](https://img.shields.io/npm/v/command-line-application.svg?style=for-the-badge)](https://www.npmjs.com/package/command-line-application) [![npm](https://img.shields.io/npm/dt/command-line-application.svg?style=for-the-badge)](https://www.npmjs.com/package/command-line-application)

A helpful wrapper around [command-line-args](https://www.npmjs.com/package/command-line-args) and [command-line-usage](https://www.npmjs.com/package/command-line-usage).

- Easily define single or multi-command CLI application
- Adds required options
- Suggests possible fixes for typos in flags or sub-commands
- Built in `--help` flag
- Add documentation footers to commands
- Automatically add color to command types
- Type checked!

## Installation

```sh
yarn add command-line-application
# or
npm i --save command-line-application
```

## Usage

### A simple single command app

```ts
import app, { Command } from 'command-line-application';

const echo: Command = {
  name: 'echo',
  description: 'Print a string to the terminal',
  examples: ['echo foo', 'echo "Intense message"'],
  required: ['value'],
  options: [
    {
      name: 'value',
      type: String,
      defaultOption: true,
      description: 'The value to print'
    }
  ]
};

const args = app(echo);

// $ echo foo
console.log(args);
// output: { value: "foo" }
```

### Complex examples

```ts
import app, { Command } from 'command-line-application';

const echo: Command = {
  examples: [{ example: 'echo foo', desc: 'The default use case' }],
  ...
};
```

### Multi Command

You can even nest multi-commands!

```ts
import app, { Command } from 'command-line-application';

const test: Command = { ... };
const lint: Command = { ... };
const scripts: MultiCommand = {
  name: 'scripts',
  descriptions: 'my tools',
  commands: [test, lint]
};

const args = app(scripts);

// $ scripts test --fix
console.log(args);
// output: { _command: 'test', fix: true }
```

### Footers

Add additional docs to your commands with Footers.

```ts
const echo: Command = {
  name: 'echo',
  description: 'Print a string to the terminal',
  examples: ['echo foo', 'echo "Intense message"'],
  options: [
    {
      name: 'value',
      type: String,
      defaultOption: true,
      description: 'The value to print'
    }
  ],
  footer: 'Only run this if you really need to',
  // or
  footer: {
    header: 'Additional Info',
    content: 'Only run this if you really need to'
  },
  // or
  footer: [
    {
      header: 'Additional Info',
      content: 'Only run this if you really need to'
    }
  ]
};
```

### Code in footers

To display code in a footer set `code` to true.

```ts
const echo: Command = {
  name: 'echo',
  description: 'Print a string to the terminal',
  examples: ['echo foo', 'echo "Intense message"'],
  options: [
    {
      name: 'value',
      type: String,
      defaultOption: true,
      description: 'The value to print'
    }
  ],
  footer: {
    header: 'Additional Info',
    code: true,
    content: 'function foo (){\n  return 1;\n}'
  }
};
```

## Options

### argv

Provide `argv` manually.

```ts
const args = app(echo, { argv: ['--help'] });
```

### showHelp

Whether to show the help dialog. Defaults to `true`

```ts
const args = app(echo, { showHelp: false });
```

### error

Configure how `command-line-application` reports errors.

- exit - (default) print error message and exit process
- throw - throw error message
- object - return the error message on an object with key `error`
