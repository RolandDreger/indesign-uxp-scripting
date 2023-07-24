# What does *not* work?
A incomplete list of what is not possible with UXP Scripting (at the moment).
## HTML
- `template` element
- Element attribute `draggable="true"`
- Element attribute `contenteditable="true"`
- [Unsupported Elements](https://developer.adobe.com/indesign/uxp/uxp/reference-html/General/Unsupported%20Elements/)
- [Unsupported Attributes](https://developer.adobe.com/indesign/uxp/uxp/reference-html/General/Unsupported%20Attributes/)

## CSS
- `display: grid`
- `gap` property for Flexbox
- `aspect-ratio`
- `@media(prefers-color-scheme: dark)` always light
- Pseudo-class `:define` (for custom elements)
- Animations do not seem to work!? (transion, @keyframes)

## JavaScript

- JavaScript alert(), confirm() or prompt() are not supported. 
- Assign a value to custom property: `Element.style.setProperty('--custom-property', 'value')`
- Get content from template element: `Template.content.cloneNode(true)`
- `setTimeout()` (I think execution is terminated before tasks in the task queue will be executed)
	With a delay of zero, only the first one seems to work in the main script:
	```
	setTimeout(() => {
		console.log("Time out finished 1");
	}, 0);
	setTimeout(() => {
		console.log("Time out finished 2");
	}, 0);
	```
- `new DOMParser()` nope ... *DOMParser is not defined*
- XPath `document.evaluate(...)` *document.evaluate is not a function* and *XPathResult is not defined*
- `new TextDecoder('utf-8')` *TextDecoder is not defined*
- `createTreeWalker()` *is not a function* 
- Property `properties` does not work anymore, e.g. `app.activeDocument.properties.fullName` // undefined
- Global function `structuredClone()` *structuredClone is not defined*

### Localization
- `require('uxp').host` is avaliable, but host.uiLocale *is undefined* (Should work, but it doesn't.)
- `navigator.language` is undefined
- `(new Intl.NumberFormat()).resolvedOptions().locale` *"en-US"* (for German UI)

## File System
- Adobe: »[...] there is no way to get a token from the file after reading using the FileSystem APIs, [...].« Currently only with the file picker.

## Spectrum
Spectrum Web Components do not seem to be registered (`window.customElements.get('sp-button')` returns *undefined*), but they are and they work a little differently than described on the [Spectrum Web Component](https://opensource.adobe.com/spectrum-web-components/) page. Also, they do not yet follow the InDesign theme changes (dark, light, ...).
- `window.customElements.get('sp-button')` *undefined*
- `window.customElements.whenDefined('sp-button')` *is not a function*
- `document.theme.getCurrent()` always "light"