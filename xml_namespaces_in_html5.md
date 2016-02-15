XML namespaces in HTML5
=======================

To be found in a dead project called [SmartWeb](https://gitlab.com/mildred593/SmartWeb/)

While trying to find a modern architecture for my static-yet-dynamic web pages, I had the problem that XML namespaces wre not present in HTML5, and custom elements are not available in XHTML5. What I was trying to do was :

- add namespacing to custom elements in HTML5
- add behaviour implementation to XML namespaces in XHTML5

The result is in [XMLBinding.js](https://gitlab.com/mildred593/SmartWeb/blob/master/XMLBinding.js). basically, I have stolen the polymer polyfill and updated it wo it worked on XHTML5. I also added some namespacing capabilities.

The HTML
--------

To use in in a HTML5 / XHTML5 document ([test.xhtml](https://gitlab.com/mildred593/SmartWeb/blob/master/test.xhtml) for example):

    <!DOCTYPE html>
    <html xmlns="http://www.w3.org/1999/xhtml" xmlns:template="tag:mildred.fr,2015-05:template#" xmlns:ds="tag:mildred.fr,2015-05:dataSource#">

The `xmlns` attribute is optional, but the other attributes describe the namespaces to be used in HTML5. Basicallt, you declare using two namespaces: `template` and `ds`. This syntax is compatible with XML namespaces and that is very cool.

    <head>
        <meta charset="utf-8"/>
        <script src="webcomponentsjs/dist/HTMLImports.js"/>

Just use the `HTMLImports.js` polyfill to get HTML5 imports.

        <script src="XMLBinding.js"/>

Use `XMLBinding.js`

        <script src="template.js"/>
        <link rel="import" href="sparql-query-data.html"/>

Use also the implementation for the two namespaces. It can either be a simple javascript or a HTML import.

    </head>
    <body>

You can then use the custom elements. Either HTML style using the `<`namespace`-`tagname`/>` notation or the XML style using `<`namespace`:`tagname`/>`:

        <ds-sparql-query src="http://localhost:8000/" id="query">
            PREFIX sw: &lt;tag:mildred.fr,2015-05:SmartWeb#&gt;
            SELECT *
            WHERE {
                GRAPH ?g {
                    ?s ?p ?o .
                }
            }
            LIMIT 10
        </ds-sparql-query>
    	
    	<template:test template:attr="value"></template:test>
    
        <template id="table-template">
            <!-- use shadow dom style templating -->
            <table template:variable="data">
                <tr>
                    <th template:select="data.head.vars" template:variable="head">
                        <template:value select="head"/>
                    </th>
                </tr>
                <tr template:select="data.results.bindings" template:variable="row">
                    <td template:select="data.head.vars" template:variable="head">
                        <template:value select="JSON.stringify(row[head].value)"></template:value>
    					<small>
                        <template:value select="JSON.stringify(row[head].value)"></template:value>
    					</small>
                    </td>
                </tr>
            </table>
        </template>
    
        <template-instance template="table-template" data-set="query"/>
    </body>
    </html>

The custom element implementation
---------------------------------

The custom elements provided by XMLBinding.js are very similar to custom elements in HTML5. [XMLBinding.js](https://gitlab.com/mildred593/SmartWeb/blob/master/XMLBinding.js) is just declaring a new function:

    registerElementNS(namespaceURI, localName, definition)

`registerElementNS` registers a custom
element in the prefix identified by namespaceURI. It also register the custom
element "`{namespaceURI}-{localName}`" in the XHTML prefix (it relies on
document.registerElement implementation for that). The `definition` parameter
must be an object that may contain:

  - `prototype`: replacement of the prototype (modification of `__poto__`) for
    the custom elements
  - `initialized`: function called to initialize the custom element
  - `attached`: function called when the custom element is attached
  - `detached`: function called when the custom element is detached

This library also implements `document.registerAttributeNS` on the same
 principles as `document.registerElementNS` (except the `prototype` in the
definition parameter is not used).

This implementation has not been tested a lot. It has been found to work on
Firefox 38. Notably, it uses DOM3 XPath that may not be available everywhere.

This implementation is freely inspired from the Polymer implementation of
`document.registerElement` and some code may have found its way here.


