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
- `aspect-ratio`
- Animations do not seem to work!? (transition, @keyframes)

## JavaScript

- Assign a value to custom property: `Element.style.setProperty('--custom-property', 'value')`
- `new DOMParser()` nope ... *DOMParser is not defined*
- XPath `document.evaluate(...)` *document.evaluate is not a function* and *XPathResult is not defined*
- `new TextDecoder('utf-8')` *TextDecoder is not defined*
- Property `properties` does not work anymore, e.g. `app.activeDocument.properties.fullName` // undefined
- Global function `structuredClone()` *structuredClone is not defined*

### Localization
- `navigator.language` is undefined (-> require("uxp").host.uiLocale works)
- `(new Intl.NumberFormat()).resolvedOptions().locale` *"en-US"* (for German UI)