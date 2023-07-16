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

```
ECMAScript 3
```

	</td>
	<td style="vertical-align: top;">

```
ECMAScript 6 | V8
```

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
				Story.paragraphs[0]
			</td>
			<td style="vertical-align: top;">
				Story.paragraphs.item(0)
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
				app.activeDocument.constructor.name // Document
			</td>
			<td style="vertical-align: top;">
				app.activeDocument.constructorName // "Document" Prior to InDesign 18.4<br>
				require("indesign").app.activeDocument.constructor.name // "Document"<br>
				require("indesign").app.activeDocument.constructorName // "Document"
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
				app.activeDocument === app.documents[0] // true
			</td>
			<td style="vertical-align: top;">
				const app = require("indesign").app;<br>
				app.activeDocument.equals(app.documents.item(0)); // true
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
				app.activeDocument instanceof Document // true
			</td>
			<td style="vertical-align: top;">
				Instead, we have to use the Constructor Name property.
			</td>
		</tr>
	</tbody>
</table>

### Global object `document` 

The global object `document` is unsupported in UXP.

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
				app.activeScript // File
			</td>
			<td style="vertical-align: top;">
				const app = require("indesign").app;<br>
				const script = await app.activeScript<br>
				script.nativePath; // "/"<br>
				script.isFolder; // true<br>
				script.scriptPath.url.href // "file:///"
			</td>
		</tr>
	</tbody>
</table>

### Passing Script Arguments

InDesign 18.4 onward: Arguments/parameters can be passed to UXP scripts. Read more in the article [Passing Arguments](https://developer.adobe.com/indesign/uxp/recipes/arguments/)

<table>
	<tbody>
		<tr>
			<th>JSX</th>
			<th>UXP</th>
		</tr>
		<tr>
			<td style="vertical-align: top;">
				app.scriptArgs.setValue("argOne", "one);<br>
				app.scriptArgs.getValue("argOne"); // "one"
			</td>
			<td style="vertical-align: top;">
				const script = require("uxp").script;<br>
				script.args.push("one");<br>
				script.args.push("two");<br>
				script.args // ["one", "two"]
			</td>
		</tr>
	</tbody>
</table>

### Logging

Standard open file dialog box

<table>
	<tbody>
		<tr>
			<th>JSX</th>
			<th>UXP</th>
		</tr>
		<tr>
			<td style="vertical-align: top;">
				$.writeln('Your log message.');
			</td>
			<td style="vertical-align: top;">
				console.log('Your log message.');<br>
				console.warn('Your log message.');<br>
				...
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
				var file = File.openDialog("Select file");
			</td>
			<td style="vertical-align: top;">
				const ufs = require('uxp').storage.localFileSystem;<br>
				const file = await ufs.getFileForOpening();
			</td>
		</tr>
	</tbody>
</table>


&nbsp;
## InDesign App Version 18.4.0 / UXP Version 6.5.0

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

`constructor.name` for InDesign DOM objects now works again:

```
require("indesign").app;
app.activeDocument.constructor.name // "Document"
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

The plugin folder is no longer available for UXP scripts.

```
const uxpStorage = require('uxp').storage;
const localFS = uxpStorage.localFileSystem;
const pluginFolder = await localFS.getPluginFolder(); // InDesign 18.4 onward: Error
```

Arguments/parameters can now be passed to UXP scripts. Read more in the article [Passing Arguments](https://developer.adobe.com/indesign/uxp/recipes/arguments/)