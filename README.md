# InDesign UXP Scripting
Collected resources, articles and tutorials around UXP Scripting in Adobe InDesign. Hints on further sources and corrections are very welcome!

## Adobe
### Reference
- [UXP for Adobe InDesign](https://developer.adobe.com/indesign/uxp)
- [JavaScript Reference](https://developer.adobe.com/indesign/uxp/uxp/reference-js/)
### Spectrum
- [Reference](https://developer.adobe.com/indesign/uxp/uxp/reference-spectrum/)
- [Web Components](https://opensource.adobe.com/spectrum-web-components/)
- [Web Components (Storybook)](https://opensource.adobe.com/spectrum-web-components/storybook/)
- [Design System](https://spectrum.adobe.com/)
- [Color palette](https://spectrum.adobe.com/page/color-palette/)
- [UI Icons](https://spectrum.adobe.com/page/icons/)
- [Web Componentes icons-ui](https://opensource.adobe.com/spectrum-web-components/components/icons-ui/)
- [Web Componentes icons-workflow](https://opensource.adobe.com/spectrum-web-components/components/icons-workflow/)
- [Spectrum Web Components Discussions](https://github.com/adobe/spectrum-web-components/discussions)
- [Spectrum Web Components Issues](https://github.com/adobe/spectrum-web-components/issues)

&nbsp;
# Articles
- [InDesign v. 18 Ships with Scripting Powered by UXP](https://blog.developer.adobe.com/indesign-v-18-ships-with-scripting-powered-by-uxp-53e5dc008f17)
- [Migrating existing JavaScript(ExtendScript) to UXP Script](https://developer.adobe.com/indesign/uxp/guides/migrating-to-UXPScript/)
- [Was bringt InDesigns neue UXP Scripting API?](https://xporc.net/2022/12/02/was-bringt-indesigns-neue-uxp-scripting-api/)
- [Getting Started with IDJS (InDesign UXP Scripting Format)](https://indiscripts.com/post/2023/01/getting-started-with-idjs-indesign-uxp-scripting-format)
- [Plans for UXP in 2023](https://blog.developer.adobe.com/plans-for-uxp-in-2023-d06091944308) New features planned for UXP v7.0 and beyond

&nbsp;
# Tutorials
## indesignblog.com (Gregor Fellenz)
- [Quickstart UXP Scripting](https://www.indesignblog.com/2022/11/quickstart-uxp-scripting/)
- [Das UXP Developer Tool](https://www.indesignblog.com/2023/01/das-uxp-developer-tool/)

&nbsp;
# Samples
- [InDesign](https://github.com/AdobeDocs/uxp-indesign/tree/main/src/pages/reference/uxp-scripting-samples) (Adobe)
- [Photoshop](https://github.com/AdobeDocs/uxp-photoshop-plugin-samples) (Adobe)
- [InDesign](https://github.com/RolandDreger/indesign-uxp-script-snippets)

&nbsp;
# Community
- [Adobe Support Community](https://community.adobe.com/t5/indesign/ct-p/ct-indesign?page=1&sort=latest_replies&lang=all&tabid=all&topics=label-uxpscripting)
- [Creative Cloud Developer Forum](https://forums.creativeclouddeveloper.com/)

&nbsp;
# What's new in InDesign App Version 18.4.0 / UXP Version 6.5.0

InDesign DOM is now available only via module:

```
const app = require("indesign").app;
console.log(app.activeDocument.name);
```

Indesign enumerators no longer available globally, e.g.:

```
const indesign = require("indesign");
const app = indesign.app;
app.scriptPreferences.userInteractionLevel = indesign.UserInteractionLevels.NEVER_INTERACT;
```

`doScript` works:

```
const indesign = require("indesign");
const app = indesign.app;
app.doScript("console.log(\"Hello UXP world!\")", indesign.ScriptLanguage.UXPSCRIPT);
```

And application events and menus are now available as well.

Finally, the prototype chain for InDesign DOM objects has changed. So you can no longer check with hasOwnProperty:

```
var app = require("indesign").app;
app.hasOwnProperty("activeDocument")	// false
Object.hasOwn(app, "activeDocument")	// false
```

Instead e.g.:
```
("activeDocument" in app)		// true
```


&nbsp;
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
- `require('path')` is avaliable. path.basename works, but path.dirname, path.sep or path.delimiter does not. (instead use window.path.dirname() and window.path.sep)
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