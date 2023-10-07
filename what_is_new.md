# What's new in ...

## UXP

### JavaScript Version | Engine

<table>
<tbody>
<tr>
<th>JSX</th>
<th>UXP</th>
</tr>
<tr>
<td style="vertical-align: top;">
ECMAScript 3
</td>
<td style="vertical-align: top;">
ECMAScript 6 | V8
</td>
</tr>
</tbody>
</table>

### Variables

<table>
<tbody>
<tr>
<th>JSX</th>
<th>UXP</th>
</tr>
<tr>
<td style="vertical-align: top;">
var, const
</td>
<td style="vertical-align: top;">
var, let, const
</td>
</tr>
</tbody>
</table>

### InDesign Collection

Collection objects returned by InDesign no longer support the subscript operator [] to access an element with a specific index. In UXP you have to use item() instead.

<table>
<tbody>
<tr>
<th>JSX</th>
<th>UXP</th>
</tr>
<tr>
<td style="vertical-align: top;">

```javascript
Story.paragraphs[0]
```

</td>
<td style="vertical-align: top;">

```javascript
Story.paragraphs.item(0)
```

</td>
</tr>
</tbody>
</table>

### InDesign DOM Object Constructor Name

<table>
<tbody>
<tr>
<th>JSX</th>
<th>UXP</th>
</tr>
<tr>
<td style="vertical-align: top;">

```javascript
app.activeDocument.constructor.name // Document
```

</td>
<td style="vertical-align: top;">

```javascript
app.activeDocument.constructorName // "Document" Prior to InDesign 18.4

const app = require("indesign").app; // Onwards InDesign 18.4
app.activeDocument.constructor.name // "Document"
app.activeDocument.constructorName // "Document"
```

</td>
</tr>
</tbody>
</table>

### InDesign DOM Object Comparison Operator

<table>
<tbody>
<tr>
<th>JSX</th>
<th>UXP</th>
</tr>
<tr>
<td style="vertical-align: top;">

```javascript
app.activeDocument === app.documents[0] // true
```

</td>
<td style="vertical-align: top;">

```javascript
const app = require("indesign").app; // Onwards InDesign 18.4
app.activeDocument.equals(app.documents.item(0)); // true
```

</td>
</tr>
</tbody>
</table>

### InDesign DOM Object instanceof 

The instanceof keyword isn't supported for InDesign DOM objects.

<table>
<tbody>
<tr>
<th>JSX</th>
<th>UXP</th>
</tr>
<tr>
<td style="vertical-align: top;">

```javascript
app.activeDocument instanceof Document // true
```

</td>
<td style="vertical-align: top;">

```javascript
// Instead, we have to use the Constructor Name property.
```

</td>
</tr>
</tbody>
</table>

### Global object `document` 

The global object `document` (which stood for an InDesign document) is unsupported in UXP.

### ActiveScript

Prior to InDesign 18.4: `app.activeScript` returns the path of the current script as a string and a error in UXP Developer Tool (UDT).

Onwards InDesign 18.4: see below.

<table>
<tbody>
<tr>
<th>JSX</th>
<th>UXP</th>
</tr>
<tr>
<td style="vertical-align: top;">

```javascript
app.activeScript // File
```

</td>
<td style="vertical-align: top;">

```javascript
const app = require("indesign").app; // Onwards InDesign 18.4
const script = await app.activeScript
script.nativePath; // "/"
script.isFolder; // true
script.scriptPath.url.href // "file:///"
```

</td>
</tr>
</tbody>
</table>

### Passing Script Arguments

InDesign 18.4 onward: Arguments/parameters can be passed to UXP scripts. Read more in the recipe [Passing Arguments](https://developer.adobe.com/indesign/uxp/recipes/arguments/)

<table>
<tbody>
<tr>
<th>JSX</th>
<th>UXP</th>
</tr>
<tr>
<td style="vertical-align: top;">

```javascript
app.scriptArgs.setValue("argOne", "one);
app.scriptArgs.getValue("argOne"); // "one"
```

</td>
<td style="vertical-align: top;">

```javascript
const script = require("uxp").script;
script.args.push("one");
script.args.push("two");
script.args // ["one", "two"]
```

</td>
</tr>
</tbody>
</table>

### Logging

<table>
<tbody>
<tr>
<th>JSX</th>
<th>UXP</th>
</tr>
<tr>
<td style="vertical-align: top;">

```
$.writeln('Your log message.');
```

</td>
<td style="vertical-align: top;">

```javascript
console.log('Your log message.');
console.warn('Your log message.');
...
```

</td>
</tr>
</tbody>
</table>

### File System

Standard open file dialog box

<table>
<tbody>
<tr>
<th>JSX</th>
<th>UXP</th>
</tr>
<tr>
<td style="vertical-align: top;">

```javascript
var file = File.openDialog("Select file");
```

</td>
<td style="vertical-align: top;">

```javascript
const ufs = require('uxp').storage.localFileSystem;
const file = await ufs.getFileForOpening();
```

</td>
</tr>
</tbody>
</table>


&nbsp;
## InDesign App Version 18.4.0 / UXP Version 6.5.0

- InDesign DOM is now available only via module:

```
const app = require("indesign").app;
console.log(app.activeDocument.name);
```

- Indesign enumerators no longer available globally, e.g.:

```
const indesign = require("indesign");
const app = indesign.app;
app.scriptPreferences.userInteractionLevel = indesign.UserInteractionLevels.NEVER_INTERACT;
```

`constructor.name` for InDesign DOM objects now works again:

```
require("indesign").app;
app.activeDocument.constructor.name // "Document"
```

- `doScript` works:

```
const indesign = require("indesign");
const app = indesign.app;
app.doScript("console.log(\"Hello UXP world!\")", indesign.ScriptLanguage.UXPSCRIPT);
```

- And application [events](https://developer.adobe.com/indesign/uxp/recipes/events/) and [menus](https://developer.adobe.com/indesign/uxp/recipes/menus/) are now available as well.

- The prototype chain for InDesign DOM objects has changed. So you can no longer check with hasOwnProperty:

```
var app = require("indesign").app;
app.hasOwnProperty("activeDocument")	// false
Object.hasOwn(app, "activeDocument")	// false
```

Instead e.g.:
```
("activeDocument" in app)		// true
```

- The plugin folder is no longer available for UXP scripts.

```
const uxpStorage = require('uxp').storage;
const localFS = uxpStorage.localFileSystem;
const pluginFolder = await localFS.getPluginFolder(); // InDesign 18.4 onward: Error
```

- Arguments/parameters can now be passed to UXP scripts. Read more in the recipe [Passing Arguments](https://developer.adobe.com/indesign/uxp/recipes/arguments/)

- InDesign now has the functionality to set result of a UXP script.

```
script.setResult("Hello World!");
```

- The `path` module is provided:

```
path.parse("~/Desktop/image.jpg") // {root: "", dir: "~/Desktop", base: "image.jpg", ext: ".jpg", name: "image"}
```

- Sectrum Web Components (BETA)

In addition to the built-in *Spectrum UXP widgets*, *Spectrum Web Components* are now supported. They are currently still in the beta phase.

**Manifest**

```
...
"featureFlags: {
	"enableSWCSupport": true
}
...

```

**Example: SP-Button**

1. Install
```
npm install @spectrum-web-components/theme
npm install @swc-uxp-wrappers/utils
npm install @swc-uxp-wrappers/button

```

2. Import
```
import "@spectrum-web-components/theme/sp-theme.js";
import "@spectrum-web-components/theme/src/themes.js";
import "@swc-uxp-wrappers/button/sp-button.js";

```

3. Usage
```
<sp-theme theme="spectrum" color="light" scale="medium" dir="ltr">
	<sp-button variant="primary">Click me</sp-button>
</sp-theme>
```