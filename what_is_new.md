# What's new in ...

## UXP



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
				<p>```var file = File.openDialog("Select file");```</p>
			</td>
			<td style="vertical-align: top;">
				const uxpfs = require('uxp').storage;<br>
				const ufs = uxpfs.localFileSystem;<br>
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