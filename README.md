chai-highlight
==============

*Chai.js* plugin to highlight values in error messages and NPM module names in
stack traces that *Chai* generates.

This plugin uses best efforts to identify objects, arrays, strings, regular expressions,
numbers, and other important elements (eg, the word *not*) comprising a *Chai*
error message, and highlight them so you can easily differentiate them from
surrounding text and inspect and compare their values.

This plugin also marks NPM module names in the accompanied stack traces, so that
you can see at a glance whether each source file in the stack trace is your own
code or from a third-party module, and in the latter case, which module.

Example
=======

This example shows *Mocha*'s error presentation with this plugin being used and
not used, side by side. When using this plugin, it's easier to quickly grasp the
essence of the error message, and it's also easier to compare the printed values
to see why the expectation is violated.

![Example](https://raw.github.com/halfninety/chai-highlight/master/example/example.png)

Note that we are using the `containAtIndex` assertion from [Chai Common](https://github.com/halfninety/chai-common)
in this example. This plugin doesn't support styling error messages generated by
the `contain` and `include` assertions in *Chai*'s core, because they are chainable
methods. See *Caveats* below for more info.


Install
=======

    npm install chai-highlight


Usage
=====

```js
var chai = require('chai'),
    expect = chai.expect,
    highlight = require('chai-highlight');

chai.use(highlight);

// Set custom styles, use HTML instead of ANSI color codes.
highlight.setStyles('<b>', '</b>', /<b>/);
highlight.setMarkStyles('<i>', '</i>');

// Use Chai as before.
expect(1).to.be.ok;
```


Custom Styles
=============

For value highlights in error messages, the default style is **bold**, which is
a sensible default because it works well with different color schemes. But you
can easily change it to anything you want, without being limited to ANSI color
codes at all.

For NPM module name markings in stack traces, the default style is a cyan
background.

Custom styles are set using the `setStyles()` and `setMarkStyles()` methods, as
illustrated in the above example.

The `setStyles()` method takes three arguments:

1. highlightStyle. The style used by highlighted text. The default is `'\x1B[1m'`.

2. restoreStyle. The sequence used to restore the style to the default for non-highlighted
text. The default is `'\x1B[22m'`.

3. stylePattern. A regular expression. If the error message matches this regex, we
don't style it. This is intended to not style error messages already styled by assertion
authors. The default is `/\x1B\[/`.

The `setMarkStyles()` method takes two arguments:

1. markStyle. The style used by marked NPM module names. The default is `'\x1B[46m'`.

2. restoreMarkStyle. The sequence used to restore the style to the default for non-marked
text. The default is `'\x1B[49m'`.

You can set any number of these in a `setStyles()` or `setMarkStyles()` call, if
a falsy value is passed in, that parameter isn't changed.


Caveats
=======

This plugin is implemented by overwriting all assertion properties and methods in the
`Assertion` object. Don't worry, for passing tests, the impact on performance should
be minimal. But the problem is that, *Chai* doesn't support overwriting chainable methods,
so error messages generated by them aren't styled.

Luckily, there are only a few of them in *Chai*'s core, including `a`, `an`, `contain`,
`include`, and `length`.

License
=======

MIT License
