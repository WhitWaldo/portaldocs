
## Overview

Extension writers provide their own HTML templates and associated Cascading Style Sheets (CSS) to render and style their experiences. Those files are analyzed at runtime to filter out disallowed HTML tags, attribute names, attribute values, CSS properties, or CSS property values that are known to have potential negative impact on the Portal. This filtering ensures a consistent and sandboxed experience in the Portal.

The philosophy is to allow as much as possible for extension to use based on a  [whitelist](portalfx-extensions-glossary-style-guide.md). As such, developers may encounter items filtered out that are not mentioned in this document. If so, issues can be reported by using the  [ibiza](https://stackoverflow.microsoft.com/questions/tagged/ibiza) tag on Stack Overflow.

For more information, see [portalfx-stackoverflow.md](portalfx-stackoverflow.md).

The criteria that determine whether a css or an html element might have a negative impact on an extension are as follows.

1. Usage allows an element to escape the tile or blade container boundaries
1. Usage significantly impacts browser performance during  layout or repainting
1. Usage can be exploited as a possible security threat
1. Usage is non-standard across IE11, Microsoft Edge, Chrome, Firefox, or Safari.
1. Usage resets defaults font-family branding that is enforced for typographic consistency.
1. Usage assigns or relies on unique identifiers. Identifier allocation is reserved in the Portal for accessibility purposes.

## HTML sanitization
S

### HTML tags

The `iframe` tag is not allowed based on criterion 3.

### HTML attributes

The `id` attribute is not allowed based on criterion 6.

### Attribute data-bind sanitization

### CSS sanitization

All CSS properties are allowed, with a few exceptions. The following properties are sanitized as described.

| Property | Criterion | Reason |
| -------- | --------- | ------- |
| position | Criterion 1 |  Only a subset of the possible values are allowed: static, relative, absolute |
| font | Criterion 5 |  Disallowed. Instead use expanded properties, like font-size, font-style, etc. |
| font-family |Criterion 5 |  Disallowed. Instead, use the portal classes that assign font-family: msportalfx-font-regular, msportalfx-font-bold, msportalfx-font-semibold, msportalfx-font-light, msportalfx-font-monospace |
| list-style | Criterion 3 |  Disallowed. Use expanded properties instead of the shorthand (list-style-type, etc) |
| user-select | Criterion 4 |  Disallowed. Use Framework class msportalfx-unselectable to achieve the same result. |

CSS media queries are not supported, and therefore are filtered out. The most common scenario for media queries is responding to container size. Extensions support this feature by subscribing to the container ViewModel tile size property as specified  in the LAYOUTDOC(LINKTOLAYOUTHERE) [portalfx-no-pdl-programming.md](portalfx-no-pdl-programming.md), [portalfx-blades-layout.md](portalfx-blades-layout.md), or [top-extensions-forms.md](top-extensions-forms.md).

## SVG sanitization

SVG sanitization follows the rules applied to HTML sanitization, because they are both DOM structures. SVG's should be converted using the build tools specified in []() previous to being used directly in the Portal.

Some SVG functionality, like certain filters, and gradients, are known to cause significant browser rendering slowdowns, as specified in Criterion 2.  As such, they are sanitized out. The build tools should detect those early and mitigate them when possible. Developers  should ask questions on 
Ask questions on: [StackOverflow](https://stackoverflow.microsoft.com/questions/tagged?tagnames=ibiza) if the sanitization of  SVG elements causes a blocking issue.