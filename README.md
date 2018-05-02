# Vega-Embed

[![npm version](https://img.shields.io/npm/v/vega-embed.svg)](https://www.npmjs.com/package/vega-embed)
[![](https://data.jsdelivr.com/v1/package/npm/vega-embed/badge)](https://www.jsdelivr.com/package/npm/vega-embed)
[![Build Status](https://travis-ci.org/vega/vega-embed.svg?branch=master)](https://travis-ci.org/vega/vega-embed)

The [Vega-Embed](http://github.com/vega/vega-embed) module provides advanced support for embedding interactive Vega views into web pages. Current version supports only [Vega](https://vega.github.io/vega) 3 / [Vega-Lite](https://vega.github.io/vega-lite) 2. The primary features include:

- Load Vega/Vega-Lite specs from source text, parsed JSON, or URLs.
- Add action links such as "View Source" and "Open in Vega Editor".
- Includes [Vega Tooltip](https://github.com/vega/vega-tooltip).
- Includes [Vega Themes](https://github.com/vega/vega-themes). **Experimental: themes are not stable yet**

<img src="screenshot.png" width="500">

Try Vega-Themes in our [Observable Demo](https://beta.observablehq.com/@domoritz/vega-lite-demo-with-vega-embed).

## Basic Example

You can import Vega-Embed from a local copy or (as shown below) [from jsDelivr](hhttps://www.jsdelivr.com/package/npm/vega-embed). Please replace `[VERSION]` with the correct [Vega](https://www.jsdelivr.com/package/npm/vega), [Vega-Lite](https://www.jsdelivr.com/package/npm/vega-lite), and [Vega-Embed](https://www.jsdelivr.com/package/npm/vega-embed) versions. We recommend that you specify the major versions (`vega@3`, `vega-lite@2`, `vega-embed@3`).

```html
<!DOCTYPE html>
<html>
<head>
  <!-- Import Vega 3 & Vega-Lite 2 (does not have to be from CDN) -->
  <script src="https://cdn.jsdelivr.net/npm/vega@[VERSION]"></script>
  <script src="https://cdn.jsdelivr.net/npm/vega-lite@[VERSION]"></script>
  <!-- Import vega-embed -->
  <script src="https://cdn.jsdelivr.net/npm/vega-embed@[VERSION]"></script>
</head>
<body>

<div id="vis"></div>

<script type="text/javascript">
  var spec = "https://raw.githubusercontent.com/vega/vega/master/docs/examples/bar-chart.vg.json";
  vegaEmbed('#vis', spec).then(function(result) {
    // access view as result.view
  }).catch(console.error);
</script>
</body>
</html>
```

Look at an example online at [Vega-Embed Block](https://bl.ocks.org/domoritz/455e1c7872c4b38a58b90df0c3d7b1b9).


## API Reference

<a href="#embed" name="embed">#</a>
<b>embed</b>(<i>el</i>, <i>spec</i>[, <i>opt</i>])
[<>](https://github.com/vega/vega-embed/blob/master/src/embed.ts "Source")

Returns a [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)* that resolves to the instantiated [Vega `View` instance](https://github.com/vega/vega-view#vega-view) and a copy of the parsed JSON Vega spec. The `embed` function accepts the following arguments:

| Property| Type       | Description    |
| :------ | :--------- | :------------- |
| `el`      |  String  | A DOM element or CSS selector indicating the element on the page in which to add the embedded view. |
| `spec`    | String / Object | _String_ : A URL string** from which to load the Vega specification. <br> _Object_ :  The Vega/Vega-Lite specification as a parsed JSON object. |
| `opt`     | Object   | _(Optional)_ A JavaScript object containing options for embedding. |

*_Internet Explorer does not support [the ES6 Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) feature. To make it work correctly, please follow [the instructions on the Vega website](https://vega.github.io/vega/usage/#ie)._

**_This URL will be subject to standard browser security restrictions. Typically this URL will point to a file on the same host and port number as the web page itself._

### Options

```js
var opt = {
  "mode": ...,

  "theme": ...,

  // view config options
  "renderer" : ...,
  "loader" : ...,
  "logLevel" : ...,

  "tooltip": : ...,

  "onBeforeParse" : ...,

  "width" : ...,
  "height" : ...,
  "padding" : ...,

  "actions" : {
    "export": ...,
    "source": ...,
    "compiled": ...,
    "editor": ...
  },

  "config"  : ...,

  "editorUrl": ...,

  "sourceHeader" : ...,
  "sourceFooter": ...,

  "hover" : ...,

  "runAsync": ...
}
```

| Property | Type             | Description    |
| :------- | :--------------- | :------------- |
| `mode`        | String        | If specified, tells Vega-Embed to parse the spec as `vega` or `vega-lite`. Vega-Embed will parse the [`$schema` url](https://github.com/vega/schema) if the mode is not specified. Vega-Embed will default to `vega` if neither `mode`, nor `$schema` are specified. |
| `theme`       | String        | If specified, tells Vega-Embed use the theme from [Vega Themes](https://github.com/vega/vega-themes). **Experimental: we may update themes with minor version updates of Vega-Embed.** |
| `renderer`    | String        | The renderer to use for the view. One of `"canvas"` (default) or `"svg"`. See [Vega docs](https://vega.github.io/vega/docs/api/view/#view_renderer) for details. |
| `logLevel`    | Level         | Sets the current log level. See [Vega docs](https://vega.github.io/vega/docs/api/view/#view_logLevel) for details. |
| `tooltip`     | Handler or Boolean or Object | Provide a [tooltip handler](https://vega.github.io/vega/docs/api/view/#view_tooltip), customize the default [Vega Tooltip](https://github.com/vega/vega-tooltip) handler, or disable the default handler. |
| `loader`      | Loader        | Sets a custom Vega loader. See [Vega docs](https://vega.github.io/vega/docs/api/view/#view) for details. |
|`onBeforeParse`| Function      | Modifies the spec before it is being parsed.|
|  `width`      | Number        | Sets the view width in pixels. See [Vega docs](https://vega.github.io/vega/docs/api/view/#view_width) for details. Note that Vega-Lite overrides this option. |
| `height`      | Number        | Sets the view height in pixels. See [Vega docs](https://vega.github.io/vega/docs/api/view/#view_height) for details. Note that Vega-Lite overrides this option. |
| `padding`     | Object        | Sets the view padding in pixels. See [Vega docs](https://vega.github.io/vega/docs/api/view/#view_padding) for details. |
| `actions`     | Boolean / Object | Determines if action links ("Export as PNG/SVG", "View Source", "View Vega" (only for Vega-Lite), "Open in Vega Editor") are included with the embedded view. If the value is `true`, all action links will be shown and none if the value is `false`.  This property can take a key-value mapping object that maps keys (`export`, `source`, `compiled`, `editor`) to boolean values for determining if each action link should be shown.  By default, `export`, `source`, and `editor` are true and `compiled` is false but these defaults can be overridden. For example, if `actions` is `{export: false, source: true}`, the embedded visualization will have two links – "View Source" and "Open in Vega Editor". |
| `config`      | String / Object | _String_ : A URL string** from which to load a [Vega](https://vega.github.io/vega/docs/config/)/[Vega-Lite](https://vega.github.io/vega-lite/docs/config.html) or [Vega-Lite](https://vega.github.io/vega-lite/docs/config.html) configuration file. <br> _Object_ : A Vega/Vega-Lite configuration as a parsed JSON object to override the default configuration options. |
| `editorUrl`    | String   | The URL at which to open embedded Vega specs in a Vega editor. Defaults to `"http://vega.github.io/editor/"`. Internally, Vega-Embed uses [HTML5 postMessage](https://developer.mozilla.org/en-US/docs/Web/API/Window/postMessage) to pass the specification information to the editor. |
| `sourceHeader` | String   | HTML to inject into the `` tag of the page generated by the "View Source" and "View Vega" action link. For example, this can be used to add code for [syntax highlighting](https://highlightjs.org/). |
| `sourceFooter` | String   | HTML to inject into the end of the page generated by the "View Source" and "View Vega" action link. The text will be added immediately before the closing `` tag. |
| `hover`       | Boolean  | Enable [hover event processing](https://vega.github.io/vega/docs/api/view/#view_hover). |
| `runAsync`     | Boolean  | Use [`runAsync`](https://vega.github.io/vega/docs/api/view/#view_runAsync) instead of [`run`](https://vega.github.io/vega/docs/api/view/#view_run). |

**_This URL will be subject to standard browser security restrictions. Typically this URL will point to a file on the same host and port number as the web page itself._

## Build Process

To build `vega-embed.js` and view the test examples, you must have [yarn](https://yarnpkg.com/en/) installed.

1. Run `yarn` in the Vega-Embed folder to install dependencies.
2. Run `yarn build`. This will invoke [browserify](http://browserify.org/) with [tsify](https://github.com/TypeStrong/tsify) to bundle the source files into `vega-embed.js`, and then [uglify-js](http://lisperator.net/uglifyjs/) to create the minified `vega-embed.min.js`.
3. Run a local webserver (e.g., `python -m SimpleHTTPServer 8000`) in the Vega-Embed folder and then point your web browser at the test page (e.g., `http://localhost:8000/test-vg.html`(Vega) or `http://localhost:8000/test-vl.html`(Vega-Lite)).

## Usage in Observable

Check out [this example](https://beta.observablehq.com/@domoritz/vega-lite-demo-with-vega-embed).
