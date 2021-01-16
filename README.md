# @macu/dynamic-stylesheet-js

Dynamically render a document stylesheet.

Make sure you sanitize all user-supplied input that is included in stylesheet output.

## Rules

Rulesets are objects containing hierarchical CSS with selectors as keys to objects, and rules represented as `'propertyName': 'value'` entries.

If a nested selector does not begin with a combinator (`>`, `+`, or `~`) or pseudo class/element syntax (`:`, `::`), it is rendered with a descendent combinator (` `). Selectors can also begin with the optional `&` cursor to append to the current selector target component (example below).

Arrays are supported; entries may be either `'propertyName: value'` strings, `/* comments */`, or nested rulesets.

## Usage

[See demo](https://macu.github.io/dynamic-stylesheet-js/demo.html)

```js
var color = 'blue';
var rules = [
	'/* comments appear in innerHTML output */',
	{'#app': {
		'>header': {
			'padding': '10px 0',
			'background-color': 'aquamarine',
			'>h1': {
				'color': 'white',
			},
		},
		'p': {
			'color': color,
			'&.demo': {
				'font-weight': 'bold',
				'text-decoration': 'underline wavy',
			},
		},
	}},
];
```

Using stylesheet API:

```js
var style = document.createElement('style');
style.type = 'text/css';
document.head.appendChild(style);
// style node must be present in the document before calling
DynamicStylesheet.addStylesheetRules(style, rules);
```

Using innerHTML:

```js
var style = document.createElement('style');
style.type = 'text/css';
// can set innerHTML before style node is present in the document
style.innerHTML = DynamicStylesheet.buildStylesheetCSS(rules);
document.head.appendChild(style);
```

Using convenience method:

```js
var style = DynamicStylesheet.appendStylesheet(document.head, rules);
```

Rendered stylesheet:

```css
<style type="text/css">
#app>header {
	padding: 10px 0;
	background-color: aquamarine;
}
#app>header>h1 {
	color: white;
}
#app p {
	color: blue;
}
#app p.demo {
	font-weight: bold;
	text-decoration: underline wavy;
}
</style>
```

## Testing

Build, start server, and access
[http://localhost:8080/demo.html](http://localhost:8080/demo.html)

```bash
npm run prod
python -m http.server 8080
```
