# What does *not* work?
A incomplete list of what is not possible with UXP Scripting (at the moment).
## HTML
- Element attribute `draggable="true"`
- Element attribute `contenteditable="true"`
- [Unsupported Elements](https://developer.adobe.com/indesign/uxp/uxp/reference-html/General/Unsupported%20Elements/)
- [Unsupported Attributes](https://developer.adobe.com/indesign/uxp/uxp/reference-html/General/Unsupported%20Attributes/)

## CSS
- `display: grid`
- `gap` property for Flexbox
- `float`
- `aspect-ratio`
- Animations do not seem to work!? (transition, @keyframes)
- ...

## JavaScript

- Assign a value to custom property: `Element.style.setProperty('--custom-property', 'value')`
- `new DOMParser()` nope ... *DOMParser is not defined*
- XPath `document.evaluate(...)` *document.evaluate is not a function* and *XPathResult is not defined*
- `new TextDecoder('utf-8')` *TextDecoder is not defined*
- Up to v18.4: Property `properties` does not work anymore, e.g. `app.activeDocument.properties.fullName` // undefined
	Onwards v18.5: Since the current version it seems to work (almost) like before. Is everything okay now? That has to be shown by further tests. One of the changes: `Document.fullName` now returns a Promise: `app.activeDocument.properties.fullName` // Promise
- Global function `structuredClone()` *structuredClone is not defined*
- Global function `window.matchMedia()` to evaluate the result of a media query. *matchMedia is not defined* 
For theme changes we can use:
```
document.theme.onUpdated.addListener((theme) => {
    console.log(theme);
  });
```
- “new AbortController()” works. But in event listeners, “controller.abort()” with the signal has no effect.
- resolve() *resolve is not defined*, e.g. `resolve("/document[0]")` does not work
- ...

### Localization
- `navigator.language` is undefined (-> require("uxp").host.locale works)
- `(new Intl.NumberFormat()).resolvedOptions().locale` *"en-US"* (for German UI)
