# [postcss][postcss]-filter-plugins [![Build Status](https://travis-ci.org/postcss/postcss-filter-plugins.svg?branch=master)][ci]

> Exclude/warn on duplicated PostCSS plugins.

## Install

With [npm](https://npmjs.org/package/postcss-filter-plugins) do:

```console
$ npm install postcss-filter-plugins --save
```

## Example

Note that this plugin does not actually transform your CSS; instead, it ensures
that plugins in the PostCSS instance are not duplicated. It is intended to be
used by plugin packs such as [cssnano] or [cssnext].

```js
var counter = postcss.plugin('counter', function () {
    return function (css) {
        css.eachDecl('foo', function (decl) {
            let value = parseInt(decl.value, 10);
            value += 1;
            decl.value = String(value);
        });
    }
});

var css = 'h1 { foo: 1 }';
var out = postcss([
    filter(),
    counter(),
    counter()
]).process(css).css;

console.log(out);
// => h1 { foo: 2 }
// Note that you will get a PostCSS warning in the message registry
```

## API

### filterPlugins([options])

#### options

##### direction

Type: `string`  
Default: `'both'`

Pass `'forward'`, `'backward'`, or `'both'` to customise the direction in which the
plugin will look in the plugins array. See the [tests] for examples on how this
works.

```js
postcss([ filter({
    direction: 'forward'
}) ]).process(css).css);
```

##### exclude

Type: `array`  
Default: `[] (empty)`

Plugins that should be excluded from the filter. Pass an array of plugin names.

```js
postcss([ filter({
    exclude: ['postcss-cssstats']
}) ]).process(css).css);
```

##### silent

Type: `boolean`  
Default: `false`

Set this to true to disable the plugin from emitting any PostCSS warnings.

```js
postcss([ filter({
    silent: true
}) ]).process(css).css);
```

##### template

Type: `function`  
Default: `format function`

This function will be passed each PostCSS plugin object. You are expected to
return a string from each call, which is then used to warn the user about her
duplicated plugins.

```js
postcss([ filter({
    template: function (plugin) {
        return 'Duplicate plugin found: ' + plugin.postcssPlugin;
    }
}) ]).process(css).css);
```

## Usage

See the [PostCSS documentation](https://github.com/postcss/postcss#usage) for
examples for your environment.

## Contributors

Thanks goes to these wonderful people ([emoji key](https://github.com/kentcdodds/all-contributors#emoji-key)):

<!-- ALL-CONTRIBUTORS-LIST:START - Do not remove or modify this section -->
<!-- prettier-ignore -->
| [<img src="https://avatars.githubusercontent.com/u/1282980?v=3" width="100px;"/><br /><sub><b>Ben Briggs</b></sub>](http://beneb.info)<br />[💻](https://github.com/postcss/postcss-filter-plugins/commits?author=ben-eb "Code") [📖](https://github.com/postcss/postcss-filter-plugins/commits?author=ben-eb "Documentation") [👀](#review-ben-eb "Reviewed Pull Requests") [⚠️](https://github.com/postcss/postcss-filter-plugins/commits?author=ben-eb "Tests") | [<img src="https://avatars.githubusercontent.com/u/157534?v=3" width="100px;"/><br /><sub><b>Maxime Thirouin</b></sub>](https://moox.io/)<br />[📖](https://github.com/postcss/postcss-filter-plugins/commits?author=MoOx "Documentation") | [<img src="https://avatars.githubusercontent.com/u/373545?v=3" width="100px;"/><br /><sub><b>Andreas Lind</b></sub>](https://github.com/papandreou)<br />[💻](https://github.com/postcss/postcss-filter-plugins/commits?author=papandreou "Code") | [<img src="https://avatars3.githubusercontent.com/u/17342435?v=4" width="100px;"/><br /><sub><b>Ryan Zimmerman</b></sub>](http://www.ryanzim.com)<br />[💻](https://github.com/postcss/postcss-filter-plugins/commits?author=RyanZim "Code") | [<img src="https://avatars0.githubusercontent.com/u/1133591?v=4" width="100px;"/><br /><sub><b>James Hunt</b></sub>](http://jamescodes.co.uk)<br />[💻](https://github.com/postcss/postcss-filter-plugins/commits?author=JamesCodes "Code") |
| :---: | :---: | :---: | :---: | :---: |
<!-- ALL-CONTRIBUTORS-LIST:END -->

This project follows the [all-contributors](https://github.com/kentcdodds/all-contributors) specification.
Contributions of any kind welcome!


## License

MIT © [Ben Briggs](http://beneb.info)

[ci]:      https://travis-ci.org/postcss/postcss-filter-plugins
[cssnano]: http://cssnano.co
[cssnext]: http://cssnext.io
[postcss]: https://github.com/postcss/postcss
[tests]:   src/__tests__
