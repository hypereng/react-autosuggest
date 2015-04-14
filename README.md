[![Build Status][status-image]][status-url]
[![NPM Version][npm-image]][npm-url]
[![NPM Downloads][downloads-image]][downloads-url]

# React Autosuggest

[WAI-ARIA compliant](http://www.w3.org/TR/wai-aria-practices/#autocomplete) React autosuggest component.

## Live Examples

* <a href="//moroshko.github.io/react-autosuggest" target="_blank">Basic example</a><br>
* <a href="//moroshko.github.io/react-autosuggest#Custom renderer" target="_blank">Custom renderer</a><br>
* <a href="//moroshko.github.io/react-autosuggest#Multiple sections" target="_blank">Multiple sections</a>

## Installation

```shell
npm install react-autosuggest --save
```

## Basic Usage

```javascript
import Autosuggest from 'react-autosuggest';

let suburbs = ['Cheltenham', 'Mill Park', 'Mordialloc', 'Nunawading'];

function getSuggestions(input, callback) {
  let regex = new RegExp('^' + input, 'i');

  setTimeout(function() {
    callback(null, suburbs.filter( suburb => regex.test(suburb) ));
  }, 300);
}
```
```xml
<Autosuggest suggestions={getSuggestions} />
```

### Options

##### suggestions (required)

Implement this function to tell `Autosuggest` which suggestions to display.

```javascript
function(input, callback) {
  ...
}
```

* `input` - Will be the value of the input field
* `callback` - Should be called once the suggestions are in hand, or error occurs.

  * Success example: `callback(null, `[\<suggestions>](#suggestions)`)`
  * Error example: `callback(new Error("Couldn't get locations"))`

<a name="suggestions"></a>
`<suggestions>` can have one of the following two formats:

* **To display a single section with no title:** `[`[\<suggestion>](#suggestion)`, `[\<suggestion>](#suggestion)`, ...]`
* **To display one or more sections with optional titles:** Array of objects with an optional `sectionName` and a mandatory `suggestions` keys, e.g.:

```javascript    
    [{
      suggestions: [<suggestion>, <suggestion>]   // This section won't have a title
    }, {
      sectionName: 'Second section',
      suggestions: [<suggestion>, <suggestion>, <suggestion>]
    }]
```

<a name="suggestion"></a>
`<suggestion>` can have one of the following two formats:

* String, e.g.: `'Mentone'`
* Object, e.g.: `{ suburb: 'Mentone', postcode: '3194' }`. This object cannot have a `suggestions` key. **You must implement [`suggestionRenderer`](#suggestionRenderer) and [`suggestionValue`](#suggestionValue) in this case.**

<a name="suggestionRenderer"></a>
##### suggestionRenderer (required when suggestions are objects)

This function will be used to render the suggestion. It should return `ReactElement` or a string.

```javascript
function(suggestion, input) {
  ...
}
```

* `suggestion` - The [\<suggestion>](#suggestion) (string or object)
* `input` - The value of the input field (e.g.: `'Men'`). If user interacts using the Up or Down keys at the moment, it will be the value of the input field **prior** to those interactions.

For example:

```javascript
function renderSuggestion(suggestion, input) { // In this example 'suggestion' is a string
  return (
    <span><strong>{suggestion.slice(0, input.length)}</strong>{suggestion.slice(input.length)}</span>
  );
}
```

```xml
<Autosuggest suggestions={getSuggestions} suggestionRenderer={renderSuggestion} />
```

<a name="suggestionValue"></a>
##### suggestionValue (required when suggestions are objects)

This function will be used to set the value of the input field when suggestion is selected. It is ignored when suggestions are strings.

```javascript
function(suggestionObj) {
  ...
}
```

For example:

```javascript
function getSuggestionValue(suggestionObj) {
  return suggestionObj.suburb + ' VIC ' + suggestionObj.postcode;
}
```

```xml
<Autosuggest suggestions={getSuggestions}
             suggestionRenderer={renderSuggestion}
             suggestionValue={getSuggestionValue} />
```

##### inputAttributes (optional)

Hash of attributes to pass to the input field. For example:

```javascript
let inputAttributes = {
  id: 'locations-autosuggest',
  name: 'locations-autosuggest',
  className: 'my-sweet-locations-autosuggest',
  placeholder: 'Enter locations...',
  value: 'Mordialloc'   // Initial value
};
```

```xml
<label htmlFor="locations-autosuggest">Where</label>
<Autosuggest inputAttributes={inputAttributes} suggestions={getSuggestions} />
```

## Styling

The `<Autosuggest />` component comes with no styles. You can use the following classes to style it:

* `react-autosuggest` - Component's wrapper. It includes both the input field and the suggestions list.
* `react-autosuggest__suggestions` - Suggestions list wrapper
* `react-autosuggest__suggestions-section` - Suggestions section wrapper (exists only when displaying multiple sections)
* `react-autosuggest__suggestions-section-name` - Suggestions section name wrapper (exists only when displaying multiple sections and `sectionName` is specified)
* `react-autosuggest__suggestion` - Single suggestion wrapper

Example: [`examples/src/Autosuggest.less`](https://github.com/moroshko/react-autosuggest/blob/master/examples/src/Autosuggest.less)

## Development

```shell
npm start
```

Now, open `http://localhost:3000/examples/dist/index.html`

## License

[MIT](http://moroshko.mit-license.org)

[status-image]: https://img.shields.io/codeship/41810250-aa07-0132-fbf4-4e62e8945e03/master.svg
[status-url]: https://codeship.com/projects/67868
[npm-image]: https://img.shields.io/npm/v/react-autosuggest.svg
[npm-url]: https://npmjs.org/package/react-autosuggest
[downloads-image]: https://img.shields.io/npm/dm/react-autosuggest.svg
[downloads-url]: https://npmjs.org/package/react-autosuggest
