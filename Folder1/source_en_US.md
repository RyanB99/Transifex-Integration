---
title: Yet another dictionary look up and text translator add-on
lang: en
---

# About

This document was translated from Japanese documents by Google translate.

This add-on (<acronym title="Yet Another Dictionary look up And Text Translator Add-on">yadatta</acronym>) is mainly used to look up a dictionary or perform automatic translation. Since you can register any sites, you can use it for various searches besides dictionaries and translations. Since you can also launch a subprocess, you can look up a dictionary on the local PC or open the current page with IE or Google Chrome. You can search by using the clipboard string or the URL of the page. You can copy selected text, page URL, page title, link URL, link text to clipboard. Since you can specify the format to be copied, you can copy it in any format such as Markdown format or html anchor format.

The operation when looking up a dictionary is following the [Dictionary Tooltip](https://addons.mozilla.org/en/firefox/addon/dictionary-tooltip/) add-on which was very easy to use.

This add-on queries the web site that accepts the GET / POST method using the selected character string, the character string of the clipboard, the URL of an open web page, etc., and sends the returned result to the tooltip, panel, side Bar, other TAB, other windows, etc., or launching the subprocess of the local PC and passing the character string as an argument. With the ability to launch a sub-process, You can do something like [AppLauncher](https://addons.mozilla.org/firefox/addon/applauncher/) and [Open With](https://addons.mozilla.org/firefox/addon/open-with/).

The web site to ask inquiries is not limited to dictionaries and translation sites, but anywhere you can use GET / POST methods such as google search, youtube etc.

![Schematic of add-on functions](graph.svg "Outline of add-on features")

Settings for the dictionary / search services, such as which character string to use, where to send it and where to display the results must be written in a text file in [JSON](https://en.wikipedia.org/wiki/JavaScript_Object_Notation) format. The syntax used in this file is simple. Please use Unicode [UTF-8](https://en.wikipedia.org/wiki/UTF-8) for the character code of the file. Shift_JIS garbled characters. There is no GUI related to the setting of the dictionary service. To edit JSON files, it is useful to use [a text editor that understand the syntax of JSON](https://www.google.com/search?q=json+editor "json editor - Google Search") such as [Notepad++](https://notepad-plus-plus.org/ "Notepad++ Home").

# Configuration file

## Example of configuration file
```json:services.json
{
    "services": [
        {
            "name": "Weblio SEL/CB -> TT",
            "description": "Weblio English-Japanese/Japanese-English Dictionary selection/clipboard -> Tooltip",
            "url": "https://ejje.weblio.jp/content/@S",
            "baseurl": "^https://ejje\\.weblio\\.jp/(content|sentence)/",
            "selector": "#main",
            "deselector": "ins, iframe, script, .hideDictWrp, .KejjeLb, #leadBtnWrp, .ad, .tab2, .btn, #h1Query, #h1Suffix, .intrstL, div.topic, .subMenuTR, .summaryR, .pin-icon-cell, #ePsdDl, .pin-icon-cell, .summaryC > table:nth-child(1) > tbody:nth-child(1) > tr:nth-child(2), .subMenuTop, .addToSlBtnCntner, #leadToVocabIndexBtnWrp, .hlt_CPRHT, .kijiFoot, #leadToSpeakingTestIndexBtnWrp",
            "css": "body {padding-top: 0px; min-width: 100px} .sentenceAudio, .contentAudio, .translateAudio {display:unset; width:50px} #main {margin: 0px 6px 0px 0px} .mainBlock {padding: unset !important; box-shadow: 0 4px #e8e1d2} div, table, td, p {width: unset !important; min-width: unset !important} .KejjeYrTxt {display: block;} .shMreIcn{display:none} .summaryTbl{float:right} .fa-volume-up::before {display:none} h2 {font-size: 115%; .lemmaAnc {color: red !important; font-size: larger;}}",
            "contextmenu": ["selection", "page"],
            "actionbutton": true,
            "tooltip": true,
            "tooltip_selector": true,
            "panel_selector": true,
            "tab_selector": true,
            "sidebar_selector": true,
            "hotkey": "Ctrl+Shift+U",
            "action": "fetch",
            "viewin": "tooltip",
            "search_when_mouseover": true,
            "//tooltip_maximized": true,
            "tooltip_width": 700,
            "tooltip_height": 425,
            "panel_width": 500,
            "panel_height": 600,
            "omnibox_keyword": "ej",
            "omnibox_default": true,
            "icon_name": "weblio_ejje"
        },
        {
            "name": "Wiktionary English",
            "url": "https://en.wiktionary.org/wiki/@S",
            "tooltip": true,
            "action": "fetch",
            "viewin": "tooltip",
            "icon_name": "wiktionary"
        },
        {
            "name": "copy to clipboard",
            "description": "copy title, url, selected text to clipboard",
            "format": "Title: @t\nURL: @u\n@s",
            "trim_selection": true,
            "contextmenu": "selection",
            "actionbutton": true,
            "tooltip": true,
            "hotkey": "Ctrl+Shift+F",
            "action": "clipboard",
            "icon_name": "clipboard"
        }
    ],
    "icons": {
        "weblio_ejje": "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAIAAACQkWg2AAAAA3NCSVQICAjb4U/gAAABlklEQVQokZWSTUojQRiGn0o6QSw1BBSdhYIbJT+gCE0GL+AFxMOI53DtCdzkCD2EIWMrDDOa2Iv4P5i4cOGUWrHrm0X3tIbZTIqi4K2qr563ql4VBAHjNA/Y/OyDAhkd+Xey9fWbByBuPMJYBTkAiV9vI9M9QuKkm+jYRMfvsnv0ehshcVbgBof7vb0d279GnB3c9Ha3e7vbdnCDONu/7u3tDA73EyMpQVcbgOmESGxO2wndnLaR2HRCQFcbI4Rkx/N5iDhz1vbKc155zpy1Efd8Hqb2xb1feqruF+cXTfcEcb874UxjC3j6/gVxpntSnF+cqvsjlgrlWV3zX64ie39p7y706rpeXbd3F/b+8uUq0jW/UJ4dtSROV33EPTQPgMmVtcmVNeCheZAtfbQUo5SubACPQbO4sOTNlIDiwtJj0AR0ZQMcIh8ILvamSxPLFRlaXUvP0zVfhnZiueJNl3BxQlBBEGzWP6XPICLxW84rZP/q3oYq7ymlEtn68euvJQAUqHwuk0AunwOXRjHLUutn//+z9AePA+VMori65AAAAABJRU5ErkJggg==",
        "wiktionary": "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAYAAAAf8/9hAAAABHNCSVQICAgIfAhkiAAAAyJJREFUOI11k8tPXGUAR8/3uJdXEcvQjgNleAlIVUIUKCqDLowkktpqrMaYand2UZuaWqMmNdiwMDGhYknZVPcmRllYrTWtTWjK+KCVEK1VKsMMOgRnGFqeM9zvfi6wXZj4+wNOfotzBP+/eqBp//49FT3dke1K67IC1wls3lxSc+78D/2H33x/CEAADAwcORkOl99fWloSFtiA1k5RVThEfp6LlBJv3SCFJM91cLVmKvYXjc27xC3ArmTy3LC1PmAxnmUhs0QikcJawcREjH17H2dlZY3+D04zODTF9yMv0x55PgJclE/1dDYmk3Ps3v0h5eXvcerUVziO4ovTP9Oz81M62htQUlJSvImXXuzirSOttO3YzrbyYAOAXJpNlN0V2sLx43sAQ+uDjdxRXMSx3qcBxaXRGI5WSCmIx+eJPFKNn/Xoe+fA3QByfGz6+nz6BrU1IbqfqOLEYBTXdcCH3qNtvH00ijE+jtZcik5TXxfE+paG+qo6AJmGqUxqAeP5HHz1Yc5+EyMRn0NrzR9TScDlyzPj5Oe5/Hg5Rm1VCGthdSUbBJDAWWMMvg8dO+6huirARx9HKczPY2EhS19vKwcPj6BcTShYjLWAhW0VwcpbAOLTf1otFes5Q9+xLgaHJrgy/jv3NYXZt7eDTGaVgYFhHmipBbshiVbq9gNyq7mUVBLfWJ7sbgFcDr3+OV2ROkLBMl54rpZDb3zHo511WAsCgeM4RbcBxti4kgqBQAjFif4Io9El6uu2ks16vHagc0PN2uDGAwuV1ZUAzRLg8thPs1oppJAYz+fZZ9qARarDIYxnaGttpKW5gNItAUAghADHQUqaNEBiajqplWJdeoBgU2EhF75+hWzWQwpJdmWdK9F3sTmD1g5ICWtZfB+lARZnr/4yN5chUHonAgFIHmq/F2sFSkvS6Rv8PRlHCcitLSHNGjczswDfaoCR3xjPTI6RdLfi2CxKgre2jPSWEfiUlhRQUVLETHIe39gVpMhMXk8MA0n9b7rnZxLJz2oqc48JSGduLk8bY1NWiNSF0V+vnvxkZCadXrwGXPtv8/8AK8A4tB2VYzcAAAAASUVORK5CYII=",
        "clipboard": "data:image/gif;base64,R0lGODlhEAAQAKUAAAQGBISGhMTGxERGROTm5CwqLKSmpGxubNTW1PT29Dw6PBQWFFRWVLy6vJSWlOzu7DQyNHx+fNze3AwODMzOzExOTKyurPz+/ERCRBweHAwKDIyKjMzKzExKTOzq7CwuLHRydNza3Pz6/Dw+PGRiZMTCxJyanPTy9DQ2NISChOTi5LSytCQiJP///wAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAACH5BAEAAC0ALAAAAAAQABAAAAaFwJZwSBQUJgwPcdkqTQKUyog5dDqEJ02IWtI4VKLWQ4Ngdh2SjOGkqJi96QN7kFie46fRwKMiWvEjGB4KJEMCE2gZcoGDI3UtHIgqGSBzjY8tHyktGwoPCnsoGJgSAHUcGhkdBAqjRAgaF0IIBoOuRCILFkOft0sGiCErEHRUQsOpEWFUQQA7"
    }
}
```

The setting of Wictionary is the simplest example. When you select a character string in a Web page, a button to start searching is displayed, and when you click the button, the Wiktionary is queried and the returned result is displayed on the tooltip panel. It can not be retrieved from the context menu or browser action. In a typical dictionary site, you can search only with this minimum setting. However, since information other than dictionary contents such as headers and menus has not been deleted from the search result page, it may be difficult to read.

In the example of Weblio, detailed settings are made, but you can search only with settings equivalent to Wiktionary. For instance, just by replacing the URL of Wiktionary example with the URL of Weblio, the search result of Weblio is displayed. However, in this example, since headers, menus, advertisements, etc. are excluded from the search result page, it is easy to see even if it is displayed on a small panel. You can also search from context menu, browser action, sidebar and hotkey.

The third example copies the title of the page, the URL and the selected text to the clipboard.

## Description of the configuration file

The top level of JSON is the following format.
```json:services.json
{
    "services": [
    ],
    "icons": {
    }
}
```

[`"services"`](#services)
: Describe the service setting such as dictionary site and search engine URL, search starting method, display destination and so on.

[`"icons"`](#icons)
: Describe the service icon. Icons are used for context menus, tool tip buttons, etc.

### "services"
The value of `"services"`is in the following format. Describe the contents of one service in one brace. Do not put a comma after the last element.
```json:services.json
{
},
{
},
{
}
```

#### `"name"`
Name of the service. String. Required. The value must be unique. If `%s` is included in the string and the string in the page is selected, `%s` will be replaced with the selected string when displayed as text in the context menu. However, when it is displayed in a place other than the context menu, `%s` is displayed as it is. If an ampersand (`&`) is included in the string, the next character of the ampersand is set as the access key in the context menu. However, when it is displayed in a place other than the context menu, it is displayed as it is. Reference: title section of [menus.create() - Mozilla | MDN](https://developer.mozilla.org/docs/Mozilla/Add-ons/WebExtensions/API/menus/create#Parameters "menus.create() - Mozilla | MDN").

#### `"description"`
Description of the service. String.

#### `"url"`
URL of the request destination by [GET / POST method](https://www.google.com/search?q=GET%2FPOST+method "GET / POST method - Google search"). Methods are specified with [`"method"`](#method). String. The following substrings in the string are replaced with the corresponding values.

* `@s`: Selected string.
* `@S`: Selected string. When nothing is selected, a clipboard character string.
* `@c`: Clipboard string.
* `@u`: The URL of the currently open page when invoked from other than the context menu. The URL of the page that is currently opened when invoked by opening the context menu on the tab of tab (the part clicking on the top of the browser to switch tabs). The URL of the link when invoked by opening the context menu on a link text in the page. The value of the tag's `src` attribute when invoked by opening the context menu on a `audio`, `image` or `video` element.
* `@v`: Decoded value of the escaped URL string of the `@u`. Reference: [decodeURI() | MDN](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/decodeURI "decodeURI() | MDN").
* `@t`: The title of the currently open page. When starting from the context menu, it becomes the title of the page or the text of the link depending on the position where the context menu is opened like `@u`.
* `@w`: The title of the window that currently has focus. (It is often the same as the title of the currently active tab.)
* `@k`: The Cookies of currently open page. A string of the form `"name1=value1; name2=value2; name2=value3"`. Reference: [Cookie - HTTP | MDN](https://developer.mozilla.org/docs/Web/HTTP/Headers/Cookie "Cookie - HTTP | MDN").
* `@r`: The Referrer of currently open page. Reference: [Document.referrer - Web APIs | MDN](https://developer.mozilla.org/docs/Web/API/Document/referrer "Document.referrer - Web APIs | MDN").
* `@l`: The last modified date and time of currently open page. Reference: [Document.lastModified - Web APIs | MDN](https://developer.mozilla.org/docs/Web/API/Document/lastModified "Document.lastModified - Web APIs | MDN").
* `@g`: A string representing the language of the contents of the current tab. Examples: ja, en, zh, etc. Reference: [tabs.detectLanguage() | MDN](https://developer.mozilla.org/docs/Mozilla/Add-ons/WebExtensions/API/tabs/detectLanguage "tabs.detectLanguage() | MDN").
* `@F`: A string representing the language of the browser UI. Examples: "ja", "en", "en-US", "fr", "fr-FR", "es-ES", etc. Reference: [NavigatorLanguage.language | MDN](https://developer.mozilla.org/docs/Web/API/NavigatorLanguage/language "NavigatorLanguage.language | MDN")
* `@G`: A string excluding the region part from the string representing the language of the browser UI. Example: "ja", "en", "fr", "es", etc. Reference: [NavigatorLanguage.language | MDN](https://developer.mozilla.org/docs/Web/API/NavigatorLanguage/language "NavigatorLanguage.language | MDN")
* `@@`: One at sign.
* For the following items, see properties in [URL - Web APIs | MDN](https://developer.mozilla.org/docs/Web/API/URL#Properties "URL - Web APIs | MDN").
* `@H`: Hash of the currently open page URL or link URL.
* `@h`: Host of the currently open page URL or link URL.
* `@d`: Hostname (domain) of the currently open page URL or link URL.
* `@o`: Origin of the currently open page URL or link URL.
* `@W`: Password of the currently open page URL or link URL.
* `@n`: Pathname of the currently open page URL or link URL.
* `@p`: Port of the currently open page URL or link URL.
* `@P`: Protocol of the currently open page URL or link URL.
* `@q`: Search of the currently open page URL or link URL.
* `@U`: Username of the currently open page URL or link URL.

If you include double quotes or backslashes in the string escape them with backslashes. The contents of the place folder in the format string are encoded using [encodeURIComponent()](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/encodeURIComponent "encodeURIComponent() - JavaScript | MDN"). However, `@s`, `@S`, `@c`, `@t`, `@w`, `@r`, `@u` or `@v` described at the beginning of the format string is not encoded. You can use this to open a selected string or clipboard string in a tab if it is a URL.

Example:
```json:services.json
{
    "name": "Wikipedia",
    "url": "https://en.wikipedia.org/wiki/@S",
```
Search for Wikipedia using the selected string or the clipboard string.

```json:services.json
{
    "name": "Google search within the site",
    "url": "https://www.google.com/search?q=@S site:@d",
```
Google search within the site using the selected string or the string of the clipboard (`@S`) and the host name of the current page (`@d`).

```json:services.json
{
    "name": "Open the selection / clipboard URL in a tab &B",
    "url": "@S",
    "contextmenu": ["selection", "page"],
    "action": "direct",
    "viewin": "new_tab"
}
```
Open the selection / clipboard URL in a tab.
```json:services.json
{
    "name": "Open the URL of the link in a tab &A",
    "url": "@u",
    "contextmenu": ["link", "tab", "image"],
    "action": "direct",
    "viewin": "new_tab"
}
```
Open the URL of the link in a tab.

#### `"content"`
Inquiry contents when the inquiry is the POST method. The methods are specified with [`"method"`](#method). String or object. Substrings in the string are replaced with corresponding values ​​with the same rules as [`"url"`](#url). When including double quotes and backslashes in `"content"`, escape them with backslashes.

Example:
```json:services.json
{
    "name": "DICT.org (POST)(TT)",
    "url": "http://www.dict.org/bin/Dict",
    "content": "Form=Dict1&Query=@S&Strategy=*&Database=*",
    "method": "post",
```
It is specified by a string.
```json:services.json
{
    "name": "DICT.org (POST)(TT)",
    "url": "http://www.dict.org/bin/Dict",
    "content": {"Form": "Dict1", "Query": "@S", "Strategy": "*", "Database": "*"},
    "method": "post",
```
Describe the same content in object format.

#### `"method"`
HTTP request method used for query. *`"get"`* or *`"post"`*. The default is *` "get" `*.

#### `"baseurl"`
[Regular expressions](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Regular_Expressions "regular expression - JavaScript | MDN") to decide whether or not to filter by [`"selector"`](#selector), [`"deselector"`](#deselector), [`"css"`](#css). Filtering is performed when the URL of the search result matches this putter. If `"baseurl"` is not specified, it performs pattern matching with the character string of [`"url"`](#url) excluding the first `@` and later. It is specified when the URL to be inquired and the URL returned from the inquiry are different. String.

Example:
```json:services.json
{
    "name": "Cambridge EJ Dictionary",
    "url": "http://dictionary.cambridge.org/search/english-japanese/direct/",
    "baseurl": "^http://dictionary\\.cambridge\\.org/dictionary/english-japanese/",
    "content": "q=@S",
    "method": "post",
```

#### `"selector"`
From the content of html returned from the inquiry, subtree below the element specified here is extracted and displayed. Do not display unmatched parts. Specify valid selectors such as class, id, and element name of [CSS](https://www.google.com/search?q=Cascading+Style+Sheets). To investigate the selector of the web page, tt is convenient to use [Examine and edit HTML](https://developer.mozilla.org/docs/Tools/Page_Inspector/How_to/Examine_and_edit_HTML "Investigate and edit HTML") in the Firefox context menu.

#### `"deselector"`
[`"selector"`](#selector) specifies the element to remove from the selected subtree. Specify a valid selector such as class, id, element name of CSS.

#### `"css"`
A style sheet to give to the query result html. You can make it easier to read by changing the font size or changing the color of the letters. String.

#### `"tooltip_allowjs"`
When [`"viewin"`](#viewin) is *`"tooltip"`*, specify whether to enable JavaScript in html displayed in tooltip. [Boolean](https://en.wikipedia.org/wiki/Boolean_data_type "Boolean data type - Wikipedia"). Default is *`false`* and JavaScript is disabled. You can not use JavaScript in the sidebar.

#### `"json_filter"`
If the result of the search returns with JSON instead of html, extract the elements necessary for display from the returned JSON and convert it to html. The value is an array of objects (hashes) and describes conversion processing. It can be used only when [`"action"`](#action) is *`"fetch"`*.
```json
{
    ...
    "json_filter": [
        {
            "jsonata": "...",
            "json2html": {...},
            "html_replace": [...]
        },
        {
            "jsonata": "...",
            "json2html": {...},
            "html_replace": [...]
        }
        ...
    ]
```
The value of `"json_filter"` is an array, describing rules in each element of the array to extract required information from JSON and convert it to html. If one conversion process is unsuccessful, the conversion process is performed using the next conversion rule. If the same site returns JSON of a different pattern in some cases, this may be used to make different conversions for each pattern. Depending on the pattern of JSON it may not be possible. When [`"jsonata"`](#jsonata) returns *`undefined`*, an empty array or an empty object or [`"json2html"`](#json2html) returns an empty string, it considers the conversion was unsuccessful and try the next transformation rule.

* <a name="jsonata"></a> `"jsonata"`

    Perform pattern matching on the JSON of the search result and extract necessary information for display. The value is a string. For this processing, [JSONata](http://docs.jsonata.org/index.html "JSONata | JSON query and transformation language") is used. The value of `"jsonata"` will be the expression argument of `jsonata()` as is. It is not necessary to write `"jsonata"` if all the data of the search result can be converted to html as it is.

    Example:

    ```json:services.json
    {
        "jsonata": "$[[0]]",
    ```
    If the search result is an array of JSON, retrieve the first element of the array and put it in another array and return it.

* <a name="json2html"></a> `"json2html"`

    Convert the search result JSON or the JSON retrieved by `"jsonata"` into html. The value is an object (hash). For this process, [json2html library](http://json2html.com/ "json2html • pure javascript HTML templating") is used.

    Example:

    ```json:services.json
    {
        "json2html": {"<>": "span", "html": "${0}", "title": "${1}", "class": "s"},
    ```
    The content of the hash becomes the first argument of `json2html.transform()` as is. If the operation succeeds, JSON is replaced with the html source of the execution result of `json2html.transform()`. If it does not succeed, the search result is not displayed as expected. Because function objects can not be described in JSON, the function definition ability of `json2html.transform()` can not be used. 

* `"html_replace"`

    Perform the same processing as [`"html_replace"`](#html_replace) for html converted from JSON. The syntax of the description is the same. Replace html after filtering with [`"jsonata"`](#jsonata) and converting with [`"json2html"`](#json2html) and then [`"css"`](#css) is added. If you do not need to replace `"html_replace"` need not be written.

    Example:

```json
{
    "name": "Google Translate any->ja [JSON] -> TT",
    "url": "https://translate.google.com/translate_a/single?client=gtx&sl=auto&tl=ja&dt=t&q=@S",
    "tooltip": true,
    "action": "fetch",
    "viewin": "tooltip",
    "css": ".s:hover{background-color: rgb(230, 236, 249);}",
    "json_filter": [
        {
            "jsonata": "$[0].($f:=function($s){$replace($s,'&','&amp;')~>$replace('<','&lt;')~>$replace('>','&gt;')~>$replace('\"','&quot;')~>$replace(/'/,'&#39;')};{'trn':$f($[0]),'src':$f($[1])})[]",
            "json2html": {"<>": "span", "html": "${trn}", "title": "${src}", "class": "s"},
            "html_replace": [["\\r\\n", "g", "<br>"],
                             ["(<br>)+\\\"", "g", "\\\""]]
        }
    ]
}
```
Since the inquiry result is returned as an array of JSON, it extracts the necessary part, converts it into html, and converts the paragraph break line break to `<br>`.

    ```json
    {
        "name": "Youdao dictionary [JSON] -> TT",
        "url": "http://dict.youdao.com/jsonapi?jsonversion=2&q=@S&dicts=%7B%22count%22%3A99%2C%22dicts%22%3A%5B%5B%22ec%22%2C%22ce%22%2C%22hh%22%2C%22phrs%22%2C%22baike%22%5D%2C%5B%22web_trans%22%5D%2C%5B%22fanyi%22%5D%5D%7D&abtest=8&xmlVersion=5.1",
        "tooltip": true,
        "action": "fetch",
        "viewin": "tooltip",
        "css": ".s:hover{background-color: rgb(230, 236, 249);}",
        "json_filter": [
            {
                "jsonata": "[ec.word.[{'k': 'return-phrase'.l.i,'v': ' UK/' & ukphone & '/, US/' & usphone & '/'},trs.tr.l.{'v': '&nbsp;&nbsp;' & i[0]}],phrs.phrs.phr.{'k': headword.l.i,'v': trs.tr.l.i},ce.word.[{'k': 'return-phrase'.l.i,'v': phone},trs.tr.l.{'v': i[0] & ' ' & i[1].`#text`}]]",
                "json2html": {"<>": "span", "html": "<b>${k}</b>: ${v}<br>"}
            },
            {
                "jsonata": "fanyi",
                "json2html": {"<>": "span", "html": "${tran}${0}", "title": "${input}", "class": "s"},
                "html_replace": [["\n", "g", "<br>"],
                                 ["\r<br>", "g", "\r\n"]]
            }
        ]
    }
    ```
An example where a site returns JSON with multiple patterns. Since the format of JSON that is returned is different when sending words and sending sentences to inquiries, describe multiple transformation rules. Reference: [ydwd](https://addons.mozilla.org/en-US/firefox/addon/ydwd/).

On the option page, you can set it to write the processing progress of `"json_filter"` to the [Browser Console](https://developer.mozilla.org/en-US/docs/Tools/Browser_Console "Browser Console - Firefox Developer Tools | MDN") for debugging.

#### `"html_replace"`
Replace the html source to be displayed with a regular expression. It can be used only when [`"action"`](#action) is *`"fetch"`*. The value is an array of arrays.

Example:
```json:services.json
{
    "html_replace": [["\\r\\n", "g", "<br>"],
                     ["(<br>)+\\\"", "g", "\\\""]],
```
The first element of each inner array is the first argument of [`RegExp()`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/RegExp "RegExp - JavaScript | MDN") function. The second element is the second argument of `RegExp()`. However, since regular expression literals can not be described in JSON, the first argument must be a string, not a regular expression literal. In other words, write as `"abc+d"` instead of `/abc+d/`. The third element of the inner array is the second argument of [`String.prototype.replace()`](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/String/replace "String.prototype.replace() - JavaScript | MDN"). That is, it works like `html.replace(new RegExp(first_element, second_element), third_element)`. It replaces in the order described in the outer array. In this example, it replaces all newline characters `\r\n` in html with `<br>` in the first substitution and delete consecutive `<br>` preceding double quotation `'"'` in the second substitution.

If search result is html, this is done for the html after filtering with [`"selector"`](#selector), [`"deselector"`](#deselector) and adding [`"css"`](#css). If the search result is JSON (if it is described in [`"json_filter"`](#json_filter)), this is done for the html after filtering by [`"jsonata"`](#jsonata) and [`"json2html"`](#json2html). And then [`"css"`](#css) is added.

#### `"contextmenu"`
In what context will you specify this service in the context menu (right click menu). A string or an array of strings or an object (hash). The possible values are *`"all"`*, *`"audio"`*, *`"browser_action"`*, *`"editable"`*, *`"frame"`*, *`"image"`*, *`"link"`*, *`"page"`*, *`"page_action"`*, *`"password"`*, *`"selection"`*, *`"tab"`*, *`"video"`*, *`"tools_menu"`* or *`"bookmark"`*. Reference: [menus.ContextType - Mozilla | MDN](https://developer.mozilla.org/Add-ons/WebExtensions/API/menus/ContextType "menus.ContextType - Mozilla | MDN").

Examples:

* When handling the selected character string in the page: *`"selection"`*
* When handling the clipboard character string: *`"page"`*
* When handling the selected character string in the page and the clipboard character string: *`["selection", "page"]`*
* When handling the URL of a page: *`"tab"`*, *`"page"`* or *`["tab", "page"]`*
* When handling the URL of the link in the page: *`"link"`*
* When handling the URL of image: *`"image"`*
* When handling URL of page, link in page, URL of image, URL of video and audio: *`["tab", "link", "audio", "image", "video"]`*

Other combinations are also possible, but there are combinations that are meaningless. When there are two or more items to be displayed on the context menu, it is added to the sub menu instead of the top level of the menu.

More complex settings can be made using the object format. The keywords that can be specified for objects are `"contexts"`, `"submenu"`, `"parentId"`, `"type"`, `"documentUrlPatterns"`, `"documentUrlPatterns"`. Reference: [menus.create() - Mozilla | MDN](https://developer.mozilla.org/Add-ons/WebExtensions/API/menus/create "menus.create() - Mozilla | MDN"). The value of `"contexts"` is the same as the value of the above array format.

* Sub menues

    Using the object format allows you to display elements in a submenu rather than at the top of the add-on context menu. Keywords are `"submenu"`, `"parentId"`.

    Form 1:

    ```json
    {
        "name": "Wikipedia",
        "contextmenu": {
            "contexts": ["selection", "page"],
            "parentId": "Encyclopedia"
        },
    ```
    Make an item of Wikipedia under a sub menu of *`Encyclopedia`* on the context menu. If there is no submenu of ID *`Encyclopedia`*, a submenu of ID *`Encyclopedia`* displayed as *`Encyclopedia`* will be automatically created at the top level of the context menu . You just need to use this if you want to display the element in the submenu of the first level.

    From 2:

    ```json
    {
        "name": "Japanese",
        "contextmenu": {"contexts": ["selection", "page"], "submenu": "submenu-top"}
    }
    ```
    Create a submenu with the ID *`submenu-top`* displayed as *`Japanese`* at the top level of the context menu.

    From 3:

    ```json
    {
        "name": "Translator",
        "contextmenu": {"contexts": ["selection", "page"],
                        "submenu": "submenu-sub", "parentId": "submenu-top"}
    }
    ```
    Create a submenu with the ID *`submenu-sub`* displayed as *`Translator`* under the submenu with the ID*`submennu-top`*. When creating submenus of two or more levels, first make a submenu using the form 2 and 3, then write the first description. A submenu with that ID must exist before referencing with `"parentId"`. The IDs of all submenus must be unique and must also be unique for all [`"name"`](#name) values.

* Special actions

    Using `"command"`, you can specify the action to be performed when you click on an element.

    ```json
    {
        "name": "Open sidebar",
        "contextmenu": {"contexts": ["page", "selection"], "command": "_execute_sidebar_action"}
    }
    ```
    Specifiable contents are as follows. Reference: [menus.create() - Mozilla | MDN](https://developer.mozilla.org/Add-ons/WebExtensions/API/menus/create#Parameters "menus.create() - Mozilla | MDN")。

    * *`"_execute_browser_action"`*: simulate a click on the extension's browser action, opening its popup if it has one
    * *`"_execute_page_action"`*: simulate a click on the extension's page action, opening its popup if it has one
    * *`"_execute_sidebar_action"`*: open the extension's sidebar (Close if already open)
    * *`"show_tooltip"`*: open the tooltip panel
    * *`"show_sidebar"`*: open the sidebar for search
    * *`"show_panel"`*: open the browser panel for search
    * *`"show_tab"`*: open the tab for search

* Separators

    You can put a separator in the context menu using the keyword `"type"`.

    ```json
    {
        "name": "separator1",
        "contextmenu": {"contexts": ["selection", "page"], "type": "separator"}
    }
    ```
    You can also specify `"checkbox"`, `"radio"`, `"normal"`, but it does not make any meaningful behavior.

* Restrict sites that display elements in the context menu

    Lets you restrict the item to apply only to documents whose URL matches one of the given match patterns. This applies to frames as well. Keywords are `"documentUrlPatterns"`、`"documentUrlPatterns"`. Reference: [Match patterns - Mozilla | MDN](https://developer.mozilla.org/en-US/Add-ons/WebExtensions/Match_patterns "Match patterns - Mozilla | MDN").

    ```json
    {
        "name": "Do something only when mozilla.org",
        "contextmenu": {"contexts": ["tab", "link"],
                        "documentUrlPatterns": ["*://*.mozilla.org/*"]}
    }
    ```

#### `"actionbutton"`
Whether to display this service on the action button next to the address bar. Boolean. The default is *` false`*.

#### `"tooltip"`
Whether to display a small icon for this service when selecting a string. Click on the pop-up icon to start the search. Boolean. The default is *` false`*.

#### `"tooltip_selector"`
Whether to display this service in the drop-down menu of the tooltip panel. Boolean. The default is *`false`*.

#### `"panel_selector"`
Whether to display this service in the browser panel drop-down menu when displaying the browser panel for search from the browser action. Boolean. The default is *`false`*. When searching from the search form on the browser panel, the result is displayed on the browser panel regardless of the value of [`"viewin"`](#viewin).

#### `"tab_selector"`
Whether to display this service in the drop-down menu of the tab when displaying the search tab from the browser action. Boolean. The default is *`false`*. When searching from the search form on the tab, the result is displayed on the tab regardless of the value of [`"viewin"`](#viewin).

#### `"sidebar_selector"`
Whether to display this service in the drop-down menu of the sidebar. Boolean. The default is *`false`*. When searching from the sidebar search form, the result is displayed in the sidebar regardless of the value of [`"viewin"`](#viewin). Therefore, sites using JavaScript etc may not be displayed as expected.

#### `"hotkey"`
Set the hot key to start the search. String. You can use any key on the keyboard and four modifier keys: `Alt`,` Ctrl`, `Meta`, and` Shift`. When you use more than one modifier keys described in this order. When the key is alphabetic, it is written in lowercase letters. When using `Shift` the value of the key describes the shifted key, not the key before shifting. For example, if you combine `Shift` and` a` it will be `Shift+A` instead of` Shift+a`, and in combination with `3` will be` Shift+# `instead of` Shift+3`. `Meta` corresponds to `Windows` key on Windows, corresponds to `Command` key on Mac. If you check the chckbox of "Display the typed key on the browser console" on the option page and type the key combination you want to use, the notation of the hot key will be displayed on the [Browser Console](https://developer.mozilla.org/en-US/docs/Tools/Browser_Console "Browser Console - Firefox Developer Tools | MDN"), so you can copy it to the configuration file. If typing does not display on the console, that key combination can not be used. It is the user's responsibility to avoid conflicts of key assignments with other add-ons or [Firefox's default keyboard shortcuts](https://support.mozilla.org/kb/keyboard-shortcuts-perform-firefox-tasks-quickly).

Example:
```json:services.json
{
    "hotkey": "Ctrl+f",
```
```json:services.json
{
    "hotkey": "Ctrl+Shift+F",
```
```json:services.json
{
    "hotkey": "Ctrl+3",
```
```json:services.json
{
    "hotkey": "Ctrl+Shift+#",
```
```json:services.json
{
    "hotkey": "Ctrl+Shift++",
```

#### `"hotkey_preventdefault"`
Cancel the default action associated with the hot key. Boolean. The default is *`false`*, not canceling. If you set the value to *`true`*, the Firefox default keyboard shortcut assigned to the corresponding hot key does not work. If the value remains *`false`*, the Firefox default behavior and the search assigned to the hot key are done at the same time.

Example:
```json:services.json
{
    "name": "Wikipedia",
    "hotkey": "Ctrl+Shift+J",
    "hotkey_preventdefault": true,
```
Search for Wikipedia with `Ctrl+Shift+J` and do not display the browser console.

#### `"commandkey"`
Set the command key to start the search. String. See [commands - Mozilla | MDN](https://developer.mozilla.org/Add-ons/WebExtensions/manifest.json/commands#Shortcut_values "commands - Mozilla | MDN") for the keys that can be used. `"commandkey"` can be used with Firefox 60 or later.

Example:
```json:services.json
{
    "commandkey": "Ctrl+Shift+F",
```

`"hotkey"` and `"commandkey"` can do almost the same but some behaviors are different. `"hotkey"` is triggered by a key event. Key events are also sent to other add-ons, so multiple add-ons expecting that key event will execute their task simultaneously. Since the key event is received by the script that runs in the contents displayed by the browser, this can not be used on `about: *` and [`addonns.mozilla.org`](https://addons.mozilla.org/firefox/ "Add-Ons for Firefox") and other special pages. `"commandkey"` is triggered by the WebExtensions API's [commands](https://developer.mozilla.org/Add-ons/WebExtensions/manifest.json/commands "commands - Mozilla | MDN") and the event is sent to the script that runs in the background. Therefore `"commandkey"` works on any page, including `about:*` and `addonns.mozilla.org`. However, special pages do not allow access to the content of the page, so you can not get selections, page URLs, etc., so this also can not be used for searching on special pages. Search on a special page is done from the context menu. Also, since the WebExtensions API exclusively assigns keys to an add-on, one key can only be used with only one add-on. Therefore, there is a possibility that key assignment conflicts with other add-ons. It is the user's responsibility to avoid conflicts of key assignments with other add-ons or [Firefox's default keyboard shortcuts](https://support.mozilla.org/kb/keyboard-shortcuts-perform-firefox-tasks-quickly).

#### `"omnibox_keyword"`
A keyword for choosing a service when performing a search from [omnibox](https://developer.mozilla.org/docs/Mozilla/Add-ons/WebExtensions/API/omnibox "omnibox - Mozilla | MDN") (address bar). String. You can use any character that can be input to the omnibox, but you should not use white spaces.

Example:
```json:services.json
{
    "name": "Wiktionary",
    "url": "https://ja.wiktionary.org/wiki/@S",
    "omnibox_keyword": "wt",
    ...
},
{
    "name": "Wikipedia",
    "url": "https://ja.wikipedia.org/wiki/@S",
    "omnibox_keyword": "wp",
    ...
}
```
You can search by entering a search term after the keyword in omnibox, such as "`qq wt complex`" or "`qq wp Mozilla`". The "`qq`" is a [keyword for the add-on](https://developer.mozilla.org/docs/Mozilla/Add-ons/WebExtensions/manifest.json/omnibox "omnibox - Mozilla | MDN"), which users can not change. If two or more extensions define the same keyword, then the extension that was installed last gets to control the keyword. You can complete with the tab key while entering the omnibox keyword. The entered search term is treated in the same way as the selected string in the page. That is, it is used to replace `@S` or `@s` in [`"url"`](#url). If you enter as "`qq wt`" without giving a search word and if [`"url"`](#url) contains `@S`, it will be replaced by the contents of the clipboard.

#### `"omnibox_default"`
This specifies the default service when you do not enter [`"omnibox_keyword"`](#omnibox_keyword) when searching from omnibox. Boolean. If the value of this key is *`true`*, that service will be the default service for searching from omnibox. If you specify *`true`* for multiple services, the behavior is undefined.

Example:
```json:services.json
{
    "name": "Wiktionary",
    "url": "https://ja.wiktionary.org/wiki/@S",
    "omnibox_keyword": "wiktionary",
    ...
},
{
    "name": "Wikipedia",
    "url": "https://ja.wikipedia.org/wiki/@S",
    "omnibox_keyword": "wikipedia",
    "omnibox_default": true,
    ...
}
```
Since '`"omnibox_default": true`' is specified for Wikipedia, Wikipedia can be searched with "`qq orange`" or "`qq wikipedia orange`". However, the keyword itself specified in [`"omnibox_keyword"`](#omnibox_keyword) can not be used as a search term in abbreviated form. In other words, "`qq wikipedia`" and "`qq wiktionary`" can not be used, and you need to enter as "`qq wikipedia wikipedia`" and "`qq wikipedia wiktionary`". If you only enter `qq wikipedia` without giving a search word and if [`"url"`](#url) contains `@S`, it will be replaced by the contents of the clipboard.

#### `"action"`
Specify where to send the search character string or what kind of processing is to be performed on the search character string. String. Required depending on the value of [`"viewin"`](# viewin). The value is *`"fetch"`*, *`"direct"`*, *`"childprocess"`*, *`"clipboard"`*, *`"modify_page"`*, *`"modify_page"`* or *`"through"`*. Specifiable values ​​are limited by combination with `"viewin"`.

* *`"fetch"`*

    Send a search string to the Web server to make inquiries. It makes an inquiry by so-called Ajax, and filters it to remove unnecessary elements in the returned result and displays it. It is mainly specified when displaying on tooltip panel or sidebar.

* *`"direct"`*

    Send a search string to the Web server to make inquiries. Opens a Web page just like a browser normally opens a Web page or submits a Web form. Filter the displayed Web page to remove unnecessary elements. It is mainly specified when displaying on tabs and browser panels.

* *`"childprocess"`*

    Send the search string to the subprocess. A character string can be passed as an argument of a command to be started as a subprocess or sent to the standard input of the command. The character string returned from the subprocess through the standard output can be displayed on the tooltip panel or the like. Programs and arguments to be started are described in [`""childprocess"`](#childprocess).

* *`"clipboard"`*

    Copy the selected character string to the clipboard or copy the URL of the open page, just like 'copy' in Firefox's default context menu. If you register on the tooltip's button it's a bit easier to copy right without clicking out the menu to copy it. Since you can specify the format when copying, you can copy in [Markdown](https://en.wikipedia.org/wiki/Markdown "Markdown - Wikipedia") format or anchor element format of html using the URL and the title of the page. The format is described in [`"format"`](#format).

* *`"modify_page"`*

    Adds or removes HTML elements to the current page. The contents to be changed are described in [`"modify_page"`](#modify_page). Search string is not used. With this feature you can do [something like](https://www.w3schools.com/howto/howto_google_translate.asp "How To Google Translate") Google Chrome built-in translation tool.

* *`"external_addon"`*

    Send search strings and other information to another add-on. The add-on performs a search and returns the result to this add-on. Use [`"external_addon"`](#external_addon) to set the information sent to another add-on.

    Reference: [runtime.onMessageExternal - Mozilla | MDN](https://developer.mozilla.org/docs/Mozilla/Add-ons/WebExtensions/API/runtime/onMessageExternal "runtime.onMessageExternal - Mozilla | MDN")

* *`"through"`*

    Do nothing. Do not send inquiry data anywhere. The character string specified in [`"text"`](#text) is regarded as the search result, and sent to the display destination specified in [`"viewin"`](#viewin). This is intended primarily for speech by SpeechSynthesis (Text-to-Speech). See [`"speech"`](#speech).

#### `"viewin"`
Specify where to display the query results. String. Values ​​are *`"tooltip"`*, *`"panel"`*, *`"tab"`*, *`"current_tab"`*, *`"other_tab"`*, *`"new_tab"`*, *`"window"`*, *`"window_new_tab"`*, *`"sidebar"`*, *`"clipboard"`*, *`"background_page"`*, *`"speech"`* or *`"console"`*.

* *`"tooltip"`*

    Embed query results in the web page currently displayed on the tab and display it like a small sub window. It is possible to move and resize. Since it is embedded in the parent page, it is affected by the style sheet of the parent page. For security reasons, even if JavaScript code is included in query result html, it will not be executed. In tooltip, on the page where [HTTP access control (CORS)](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) is applied, in the domain of the domain different from the HTML body of the dictionary If you display a dictionary site requesting CSS, the CSS can not be read due to CORS restriction, so the display may collapse considerably.

* *`"panel"`*

    Display on a panel independent of the parent page. Entities are windows with only one tab. It is not affected by the stylesheet of the parent page. You can also run JavaScript and so on as it would appear on tabs. You can also display sites under HTTP access control (CORS). You can specify the type of panel with [`"panel_type"`](#panel_type).

* *`" tab"`*

    Display on the tab specified on the option page (*`"current_tab"`*, *`"other_tab"`*, *`"new_tab"`*, *`"window"`* or *`"window_new_tab"`*). For pages opened in tabs, you can filter by using addons like [Stylus](https://addons.mozilla.org/en/firefox/addon/styl-us/).

* *`"current_tab"`*

    Display on the currently displayed tab.

* *`"other_tab"`*

    Open new tab and display it. The second and subsequent times are displayed on the same tab.

* *`"new_tab"`*

    Open a new tab every time and display it.

* *`"window"`*

    Open a new window and display it. The second and subsequent times are displayed on the same tab of the window.

* *`"window_new_tab"`*

    Open a new tab and display it. After the second time, open a new tab every time in the window and display it.

* *`"sidebar"`*

    Display in the sidebar. If the [`"action"`](#action) is *`"fetch"`*, the JavaScript code contained in the page will not be executed. The sidebar must be opened by the user himself before searching. It can not be opened from the add-on except starting from context menu.

* *`"browser_action"`*

    Display search results in a popup panel that opens by clicking the browser action button next to the address bar. Since you can open the browser action panel only if you invoke the search from the context menu, you can only do a search from the context menu or from a button in the browser action panel. If [`"action"`](#action) is *`"direct"`*, filtering with `"selector"`, `"deselector"`, `"css"` can not be used.

* *`"background_page"`*

    Insert as an iframe element in the add-on's background page. Background pages are not visible to the user. This is primarily intended for audio playback. Since there is no way to start or stop the audio playback, the inserted page must start playing automatically when the page loads, and it should not loop playing. 

    Example:

    ```json:services.json
    {
        "name": "Google translate English TTS",
        "url": "https://translate.google.com/translate_tts?ie=UTF-8&q=@S&tl=en&client=tw-ob",
        "tooltip": true,
        "action": "direct",
        "viewin": "background_page"
    },
    ```

* <a name="speech"></a> *`"speech"`*

    Read text using SpeechSynthesis (Text-to-Speech; TTS). Reference: [Web Speech API](https://developer.mozilla.org/docs/Web/API/Web_Speech_API "Web Speech API-Web API | MDN"). Settings for TTS are described in [`"speech"`](#speech). Specify *`"through"`* for [`"action"`](#action).

* *`"console"`*

    Display the HTML source or character string of the search result on the [Browser Console](https://developer.mozilla.org/en-US/docs/Tools/Browser_Console "Browser Console - Firefox Developer Tools | MDN").

The combination of `"viewin"` and `"action"` are as follows.

|                 | fetch | direct | childprocess | clipboard | modify_page | external_addon |
|-----------------|:-----:|:------:|:------------:|:---------:|:-----------:|:--------------:|
| tooltip         | ✓     | ✓ \*1  | ✓            | -         | -           | ✓              |
| sidebar         | ✓     | ✓ \*2  | ✓            | -         | -           | ✓              |
| panel           | ✓     | ✓      | ✓            | -         | -           | ✓              |
| tab             | ✓     | ✓      | ✓            | -         | -           | ✓              |
| current_tab     | ✓     | ✓      | ✓            | -         | -           | ✓              |
| other_tab       | ✓     | ✓      | ✓            | -         | -           | ✓              |
| new_tab         | ✓     | ✓      | ✓            | -         | -           | ✓              |
| window          | ✓     | ✓      | ✓            | -         | -           | ✓              |
| window_new_tab  | ✓     | ✓      | ✓            | -         | -           | ✓              |
| browser_action  | ✓     | ✓ \*3  | ✓            | -         | -           | ✓              |
| background_page | ✓     | ✓      | ✓            | -         | -           | ✓              |
| speech          | ✓     | -      | ✓            | -         | -           | ✓              |
| clipboard       | ✓     | -      | ✓            | -         | -           | ✓              |
| console         | ✓     | -      | ✓            | -         | -           | ✓              |
| No `"viewin"`   | -     | -      | ✓            | ✓         | ✓           | -              |
Table: Combination of `"viewin"` and `"action"`

\*1: There are cases where it is possible and impossible depending on the search site. Filtering by `"selector"`, `"deselector"`, `"css"` can not be used.

\*2: Filtering by `"selector"`, `"deselector"`, `"css"` can not be used.

\*3: Filtering by `"selector"`, `"deselector"`, `"css"` can not be used.

#### `"external_addon"`
Describe the add-on to be called when [`"action"`](#action) is *`"external_addon"`* and the information to be sent to the add-on. Object (hash). The keywords that can be specified for the object are *`"id"`*, *`"send_keys"`*, *`"options"`*.

Reference: [runtime.onMessageExternal - Mozilla | MDN](https://developer.mozilla.org/docs/Mozilla/Add-ons/WebExtensions/API/runtime/onMessageExternal "runtime.onMessageExternal - Mozilla | MDN")

Example:
```json:services.json
{
    "name": "Mirai Translate E-J",
    "content": "input=@S&source=en&target=ja&profile=nmt&kind=nmt&bt=false",
    "action": "external_addon",
    "external_addon": {
        "id": "{8ac3b0a2-0a85-4a64-a795-49cf41d0891d}",
        "send_keys": ["content", "title"],
        "options": {
            "guess_lang": true,
            "second_lang": "en"
        }
    },
    ...
```

* `"id"`

    The ID of the add-on to call. This value is specific to the add-on and will be described in the add-on documentation or somewhere. String.

* `"send_keys"`

    Describe the type of information to be passed to the add-on to be called. Array of strings. The values ​​that can be specified are as follows.

    * `"selection"` The string selected in the page.
    * `"url"` URL of the currently open page.
    * `"title"` The title of the currently open page.
    * `"referrer"` The referrer of the currently open page.
    * `"lastModified"` The date and time of the last update of the currently open page.
    * `"service"` Information about the currently processing service in the service configuration file.
    * `"cookies"` The cookie of the currently open page.
    * `"clipboard"` Clipboard contents.
    * `"window_title"` The title of the window that currently has focus. (Often the same as the title of the currently active tab.)
    * `"page_lang"` A string representing the language of the content of the currently open page. Example: ja, en, zh, etc.
    * `"query_text"` The string selected in the page. If nothing is selected, the string on the clipboard.
    * `"service_url"` The character string after performing the necessary replacement for the placeholders of the [`"url"`](#url) in the service configuration information.
    * `"content"` The character string after performing the necessary replacement for the placeholders of the [`"content"`](#content) in the service configuration information. The value is converted to a hash table.

    The information is passed to the calling add-on in the form of a hash with these names as keys and included in the hash of the `"data"` key.

* `"options"`

    Describes information specific to the add-on, such as changing the behavior of the called add-on. Any data type supported by JSON can be described. This information is passed to the add-on as is.

    Example of information passed to the add-on:

    ```json
    {
        "data": {
            "content": {
                "input": "The Brothers Poem or Brothers Song is a series of lines of verse attributed....",
                "source": "en",
                "target": "ja",
                "profile": "nmt",
                "kind": "nmt",
                "bt": "false"
            },
            "title": "Brothers Poem - Wikipedia"
        },
        "options": {
            "guess_lang": true,
            "second_lang": "en"
        }
    }
    ```

The called add-on must return an object with the following keys or *`null`*. If *`null`* is returned, the query result will not be displayed anywhere.

* `type`: The value is *`"text"`* or *`"json"`*。
* `data`: When `type` is *`"text"`*, the value is HTML text that can be displayed in a tooltip or tab. When `type` is *`"json"`*, the value is any object. This object should be converted to HTML using [`"json_filter"`](#json_filter).

Example:
```json
{
    "type": "json",
    "data": {
        "output": "The Brothers Poem or Brothers Songは、古代ギリシャの詩人...."
    }
}
```

#### `"text"`
When [`"action"`](#action) is *`"through"`*, specify the character string to be passed as the search result to the item specified in [`"viewin"`](#viewin). String. The placeholder in the string is replaced with the corresponding value according to the same rules as [`"url"`](#url).

#### `"speech"`
When [`"viewin"`](#viewin) is *`"speech"`*, describe the settings for text-to-speech using SpeechSynthesis (Text-to-Speech). Object. The keywords that can be specified for objects are `"command"` and `"utterance"`.

* `"command"`

    Specify the command to be executed by SpeechSynthesis. String. If `"command"` is omitted, it is assumed that *`"speak"`* is specified.

    * *`"speak"`* Perform speech.
    * *`"cancel"`* Stop speech.
    * *`"pause"`* Pause speech.
    * *`"resume"`* Resume paused speech.
    * *`"pause/resume"`* If the speech is being performed, the speech is paused, and if the speech is paused, the speech is resumed.
    * *`"speak/pause/resume"`* If the speech is being performed, the speech is paused, and if the speech is stopped, the speech is resumed. If not, read the new text.
    * *`"speak/cancel"`* If the speech is being performed, the speech is canceled, and if not, the new text is read.

* `"utterance"`

    Describe the settings of SpeechSynthesis (Text-to-Speech; Speech synthesis) API. Object. Keywords that can be specified for objects are `"lang"`, `"pitch"`, `"rate"`, `"volume"`, `"voice"`. These are the same properties as [SpeechSynthesisUtterance](https://developer.mozilla.org/docs/Web/API/SpeechSynthesisUtterance "SpeechSynthesisUtterance-Web APIs | MDN") and are passed to the API as is, except `"voice"`. The API's `voice` property does not need to be specified, but if specified, describe the name of the installed speech synthesis language below. API `text` property cannot be specified. The result of the search query is used for the text to be read. The result of the query should be plain text, not HTML. If *`"lang"`* is not specified, the language is determined by automatically judging the contents of the text. However, if the character string is short, it is easy to fail the judgment, so if your purpose is to hear the pronunciation of words, you should specify *`"lang"`*. *`"pitch"`*, *`"rate"`*, *`"volume"`* need not be specified if the default is acceptable. You can also stop reading by sending an empty string instead of executing the *`"cancel"`* command.

If `"speech"` is omitted, play it as if `"command"` is *`"speak"`*. Text-to-Speech is not a main purpose of this add-on, so the function of this Text-to-Speech is simple, there is no function to cancel, pause or resume audio playback . If you want to use it more conveniently, you should use a special add-on for TTS. If the OS does not have the TTS function of the language, it will not work. Reference: [How to download Text-to-Speech languages for Windows 10 - Office Support](https://support.office.com/en-us/article/how-to-download-text-to-speech-languages-for-windows-10-d5a6b612-b3ae-423f-afa5-4f6caf1ec5d3?omkt=en-US&ui=en-US&rs=en-US&ad=US "How to download Text-to-Speech languages for Windows 10 - Office Support"), [Appendix A: Supported languages and voices in Narrator in Windows 10 - Windows Help](https://support.microsoft.com/en-us/help/22805/windows-10-supported-narrator-languages-voices "Appendix A: Supported languages and voices in Narrator in Windows 10 - Windows Help").

Installed speech synthesis languages:

<div id="tts_langs"></div><script src="../misc.js"></script>

Examples:
```json
{
    "name": "🔊 Speak English",
    "description": "Speak English by built-in TTS",
    "tooltip": true,
    "contextmenu": ["selection", "page"],
    "commandkey": "Alt+Ctrl+S",
    "action": "through",
    "viewin": "speech",
    "text": "@S",
    "speech": {
        "command": "speak",
        "utterance": {
            "volume": 1,
            "rate": 1,
            "pitch": 1,
            "voice": "Microsoft Zira Desktop - English (United States)",
            "lang": "en-US"
        }
    }
},
{
    "name": "🔊 Speak auto detect",
    "description": "Readout by built-in TTS. Language is automatically determined.",
    "tooltip": true,
    "contextmenu": ["selection", "page"],
    "action": "through",
    "viewin": "speech",
    "text": "@S",
    "speech": {
        "command": "speak"
    }
},
{
    "name": "Speach RFC1866",
    "description": "Read out plain text of RFC1866",
    "contextmenu": ["selection", "page"],
    "action": "fetch",
    "url": "https://tools.ietf.org/rfc/rfc1866.txt",
    "viewin": "speech"
},
{
    "name": "Speech date",
    "description": "Read the execution result of the subprocess",
    "contextmenu": ["selection", "page"],
    "action": "childprocess",
    "childprocess": {
    "args": "date.exe /t",
    "htmlize": false,
    "stream": "@S\n"
    },
    "viewin": "speech"
},
{
    "name": "🔇 Cancel Speech",
    "description": "Send an empty string to cancel reading",
    "tooltip": true,
    "contextmenu": ["selection", "page"],
    "action": "through",
    "viewin": "speech",
    "text": "",
    "speech": {
        "command": "speak"
    }
},
{
    "name": "🔇 Cancel Speech2",
    "tooltip": true,
    "contextmenu": ["selection", "page"],
    "action": "through",
    "commandkey": "Alt+Ctrl+C",
    "viewin": "speech",
    "speech": {
        "command": "cancel"
    }
},
{
    "name": "⏸ Pause Speech",
    "tooltip": true,
    "contextmenu": ["selection", "page"],
    "action": "through",
    "viewin": "speech",
    "speech": {
        "command": "pause"
    }
},
{
    "name": "⏵ Resume Speech",
    "tooltip": true,
    "contextmenu": ["selection", "page"],
    "action": "through",
    "viewin": "speech",
    "speech": {
    "command": "resume"
    }
},
{
    "name": "⏯ Pause/Resume Speech",
    "tooltip": true,
    "contextmenu": ["selection", "page"],
    "commandkey": "Alt+Ctrl+R",
    "action": "through",
    "viewin": "speech",
    "speech": {
        "command": "pause_resume"
    }
},
```

#### `"webRequest"`
Process header and contents of HTTP request. This can be used only when [`"viewin"`](#viewin) is *`"tooltip"`* and [`"action"`](#action) is *`"direct"`*. Object (hash). The keywords that can be specified for the object are [`"onHeadersReceived"`](#onHeadersReceived) and [`"urls"`](#urls). Reference: [webRequest | MDN](https://developer.mozilla.org/docs/Mozilla/Add-ons/WebExtensions/API/webRequest "webRequest | MDN").

* `"onHeadersReceived"`

    Process the response headers from the server for the search request before the browser interprets them. Object (hash). The object's key is the name of the response header to be processed written in **lower-case letters**. The value corresponding to the key is the value newly set in the response header or *`false`*. If *`false`* is specified as the value, delete that header. Reference: [webRequest.onHeadersReceived | MDN](https://developer.mozilla.org/docs/Mozilla/Add-ons/WebExtensions/API/webRequest/onHeadersReceived "webRequest.onHeadersReceived | MDN")

* `"urls"`

  The URL to be processed. Array. Describe the URL you want to process, according to the [Match patterns](https://developer.mozilla.org/docs/Mozilla/Add-ons/WebExtensions/Match_patterns "Match patterns in extension manifests | MDN"). `"WebRequest"` is processed only for URLs that match any of the elements of this array. If omitted, the default value is the [origin](https://developer.mozilla.org/docs/Web/API/URL#Properties "URL - Web APIs | MDN") part of the value of [`"url"`](#url) concatenating `/*`. Reference: [webRequest.RequestFilter | MDN](https://developer.mozilla.org/docs/Mozilla/Add-ons/WebExtensions/API/webRequest/RequestFilter "webRequest.RequestFilter | MDN")。

Example:
```json:services.json
{
    "name": "Google Translate MOBILE -> TT",
    "url": "https://translate.google.com/m/translate#auto/ja/@S",
    "action": "direct",
    "viewin": "tooltip",
    "webRequest": {
        "onHeadersReceived": {
            "x-frame-options": false
        }
    },
    "contextmenu": ["selection", "page"],
    "tooltip": true,
    "tooltip_selector": true,
    "icon_name": "google_translate"
},
```
Display Google translation on tooltip. If you try to display the results of Google Translate in tooltip, the error `Load denied by X-Frame-Options: ...` is displayed on the [Browser Console](https://developer.mozilla.org/en-US/docs/Tools/Browser_Console "Browser Console - Firefox Developer Tools | MDN") and the browser denys to display the translation result. So use `"webRequest"` to remove the` x-frame-options` header from the response headers. Reference: [X-Frame-Options | MDN](https://developer.mozilla.org/docs/Web/HTTP/Headers/X-Frame-Options "X-Frame-Options | MDN")。

#### `"panel_type"`
[`"viewin"`](#viewin) specifies the type of panel when *`"panel"`*. String. *`"normal"`*, *`"popup"`*, *`"panel"`* or *`"detached_panel"`*. *`"popup"`*, *`"panel"`* and *`"detached_panel"`* seems to have no difference. Reference: [windows.CreateType - Mozilla | MDN](https://developer.mozilla.org/en-US/Add-ons/WebExtensions/API/windows/CreateType "windows.CreateType - Mozilla | MDN"). If `"panel_type"` is not described, it follows the option page's specification.

#### `"format"`
Specify the format when copying to the clipboard. String. Substrings in the string are replaced with corresponding values ​​with the same rules as [`"url"`](#url). When including double quotes and backslashes in `"format"`, escape them with backslashes.

Example:
```json:services.json
{
    "name": "selection -> clipboard",
    "action": "clipboard",
    "format": "\"@s@@@t\"",
    "tooltip": true,
    "contextmenu": "selection"
```
Enclose the title of the page and the selected character string in double quotes and copy it in the format `"selected_string@title"`.
```json:services.json
{
    "name": "URL -> clipboard (Markdown)",
    "action": "clipboard",
    "format": "[@t](@u \"@t\")",
    "contextmenu": ["tab", "link", "audio", "image", "video"],
```
Copy page title and URL to clipboard in Markdown format. When opening the context menu on the link text in the page, the link text and URL are copied. When opening a context menu on an image, audio or video element, the URL of src attribute of that element and the title of the page are used.
```json:services.json
{
    "name": "URL -> push to clipboard (href)",
    "action": "clipboard",
    "format": "@c\n<a href=\"@u\">@t</a>",
    "contextmenu": ["tab", "link", "audio", "image", "video"],
```
**Add** instead of copying page title and URL or link text and link URL to clipboard in html anchor format. If you want to copy may page URLs or many link URLs to a text editor etc., youd do not need to repeat copying and pasting one by one, add them to the clipboard continuously, and paste it in the text editor only once at the end. However, you need to clear the clipboard in some way first. For example, create a service with settings such as `"format": ""`.

#### `"trim_selection"`
Before searching, delete the leading and trailing white space characters of the selected character string. Boolean. The default is *` false`*.

#### `"trim_clipboard"`
Delete leading and trailing blank characters when searching using the clipboard character string. The content of the clipboard itself is not changed. Boolean. The default is *` false`*.

#### `"childprocess"`
When the value of [`"action"`](#action) is *`"childprocess"`*, describe the program name and arguments and other parapeters to start the subprocess. Value is an object.  The keywords of the object are `"args"`, `"stream"`, `"nowait"`, `"stdin_encoding"`, `"stdout_encoding"`, `"stderr_encoding"`, `"response_type"`。

* `"args"`

    Describe the program and arguments for starting the subprocess. Substrings in the string of `"args"` are replaced with corresponding values ​​with the same rules as [`"url"`](#url). When including double quotes and backslashes in `"args"`, escape them with backslashes.

    The value of `"args"` can be a single string or array format. If written in an array format, the program is started directly. If written in a single character string, it is invoked through a shell. Therefore, when using a single character string, you can use environment variables and so on.

    Example:

    ```json:services.json
    {
        "name": "Google Chrome (URL) windows direct",
        "action": "childprocess",
        "childprocess": {
            "args": ["C:\\Program Files (x86)\\Google\\Chrome\\Application\\chrome.exe", "@u"],
            "nowait": true
        },
        "contextmenu": "tab",
    ```
    When using Windows native Python. Description in array format. Start Google Chrome with the URL of the page as an argument. Open an open page in Google Chrome.

    ```json:services.json
    {
        "name": "notepad",
        "action": "childprocess",
        "childprocess": {
            "args": "\"%SystemRoot%\\System32\\notepad.exe\" \"%USERPROFILE%\\Documents\\@s.txt\"",
            "nowait": true
        },
        "contextmenu": "tab",
    ```
    When using Windows native Python. Described in a single string. I used the environment variable to specify the path of notepad.exe and the path of the file to open. On Windows, it expands to `"C:\\windows\\System32\\notepad.exe" "C:\\Users\\_username_\\Documents\\_selection_.txt"`. For Linux and Mac, you can make a description that matches each shell.

    ```json:services.json
    {
        "name": "Internet Explorer (URL) cygwin direct",
        "action": "childprocess",
        "childprocess": {
            "args": ["/cygdrive/c/Program Files/Internet Explorer/iexplore.exe", "@u"],
            "nowait": true
        },
        "contextmenu": "tab",
    ```
    When using Cygwin's Python. Start Internet Explorer with the URL of the page as an argument. The description of the path is different from that of Windows native Python.

    ```json:services.json
    {
        "name": "copy url and title into file cygwin shell",
        "action": "childprocess",
        "childprocess": {
            "args": "/bin/echo '<a href=\"@u\">@t</a>' >> ~/url.txt"
        },
        "contextmenu": "tab",
    ```
    When using Cygwin's Python. Format the URL and title of the page into HTML anchor format and add it to the file called `url.txt` under the Cygwin's home directory. The description of the path is different from that of Windows native Python. When describing with a single character string, since the shell evaluates, it is necessary to escape spaces and the like. The arguments are evaluated and executed by the Cygwin shell, so redirects, "`~`" and environment variables etc can be used.

* `"stream"`

    Describe the character string given to the standard input of the process to be started up. String. Substrings in the string of `"stream"` are replaced with corresponding values ​​with the same rules as [`"url"`](#url).

* `"nowait"`

    Specifies whether the parent process (Python) waits for the child process to terminate. Boolean. If the value is *`true`*, the parent process terminates immediately without waiting for the child process to end. When the value is *`false`*, it waits for the end of the child process. The default is *`false`*. For programs that run for a long time after startup, such as Google Chrome or a text editor, and do not need to receive output from the process, the parent process is unnecessary and the value is set to *`true`*. You do not need to set the value to *`true`*, but you can reduce the extra process and slightly reduce memory by setting it to *`true`*. If you set it to *`true`*, the add-on does not receive even error messages, so it is better to keep it *`false`* during testing the configuration. When receiving the output of the child process and displaying it on the browser, it is necessary to relay the data to the parent process, so set the value to *`false`*.

* `"stdin_encoding"`

    Encoding of the string passed to the standard input of the subprocess. String. It is described by Pyhton's notation. See [7.2. codecs — Codec registry and base classes — Python 3.6.5 documentation](https://docs.python.org/3/library/codecs.html#standard-encodings "7.2.3. Standard Encodings") for valid values. The default is *`"utf-8"`*.

* <a name="stdout_encoding"></a> `"stdout_encoding"`

    Encoding of the character string that the subprocess outputs to standard output. String. It is described by Pyhton's notation. See [7.2. codecs — Codec registry and base classes — Python 3.6.5 documentation](https://docs.python.org/3/library/codecs.html#standard-encodings "7.2.3. Standard Encodings") for valid values. The default is *`"utf-8"`*. Required when receiving output from subprocess and displaying it on browser. It may garble if it is not set correctly.

* `"stderr_encoding"`

    Encoding of the character string that the subprocess outputs to standard error output. String. It is described by Pyhton's notation. See [7.2. codecs — Codec registry and base classes — Python 3.6.5 documentation](https://docs.python.org/3/library/codecs.html#standard-encodings "7.2.3. Standard Encodings") for valid values. The default is the value of [`"stdout_encoding"`](#stdout_encoding). Error messages written to the standard error output by the subprocess are displayed in the [Browser Console](https://developer.mozilla.org/en-US/docs/Tools/Browser_Console "Browser Console - Firefox Developer Tools | MDN"). If you do not set it correctly, it will garble and you can not read the error message.

* `"htmlize"`

    When the string returned from the subprocess is plain text, it is converted into simple HTML. Boolean. The default is *`false`*, not converted to HTML. It is specified when receiving output from the subprocess and displaying it on the browser. If the value is *`"true"`*, escape "`&`", "`<`" and "`>`" in the string and enclose the whole in "`<pre>`" and "`</pre>`" tags before sending it to the display routine. When subprocess returns information in HTML format, you do not need to set this.

If the subprocess outputs something to the standard output and the service's [`"viewin"`](#viewin) is not specified, output from the subprocess is displayed on the [Browser Console](https://developer.mozilla.org/en-US/docs/Tools/Browser_Console "Browser Console - Firefox Developer Tools | MDN").

If an error occurs when spawning a subprocess, a log file may be created in the temporary folder. The file name is probably `%TEMP%\yadatta_app.log` on Windows and probably `$TMP/yadatta_app.log` on Linux. [`%TEMP%`](https://www.google.com/search?q=%25TEMP%25+Windows) is an environment variable for Windows. [`$TMP`](https://www.google.com/search?q=%24TMP+Linux) is a Linux environment variable.

On Windows, if you want to use the icon of the program to be launched as a subprocess for context menu's icon and so on, try to decompress the program (.exe) with [7-Zip](https://www.7-zip.org/ "7-Zip") etc, it may be found in the `.rsrc/ICON/` folder.

Examples:
```json:services.json
{
    "name": "EBwinC",
    "action": "childprocess",
    "childprocess": {
        "args": "\"C:\\Program Files\\EBWin4(x64)\\ebwinc.exe\" /G=English /C=1 /#=2 /O=html /M=p /S=@S",
        "stdin_encoding": "shift_jis",
        "stdout_encoding": "shift_jis",
        "htmlize": false
    },
    "contextmenu": ["selection", "page"],
    "actionbutton": true,
    "tooltip": false,
    "viewin": "tooltip",
    "icon_name": "ebwin"
}
```
Using a program called `ebwinc.exe` for searching dictionaries on the command line that comes with the EPWING / EBook (EB) viewer for Windows [EBWin4](http://ebstudio.info/manual/EBWin 4/EBWin 4.html "EBWin 4"), search the dictionary in the [EPWING](https://www.google.com/search?q=EPWING) format and display the result in the tool tip panel. `ebwinc.exe`'s character code is `shift_jis` so `"stdin_encoding"` and `"stdout_encoding"` are set to *`"shift_jis"`*. `ebwinc.exe` can output HTML if option is specified, so `"htmlize"` will be set to *`false`* and it will be displayed as it is without modification.

```json:services.json
{
    "name": "MeCab",
    "action": "childprocess",
    "childprocess": {
        "args": "\"C:\\Program Files (x86)\\MeCab\\bin\\mecab.exe\" --eos-format= --node-format=\"%m\\t%f[7]\\t%f[6]\\t%F-[0,1,2,3]\\t%f[4]\\t%f[5]\\n\" \"--unk-format=%m\\t%m\\t%m\\t%F-[0,1,2,3]\\t\\t\\n\" \"--eos-format=EOS\\n\"",
        "stdin_encoding": "shift_jis",
        "stdout_encoding": "shift_jis",
        "htmlize": true
        "stream": "@S\n"
    },
    "contextmenu": ["selection", "page"],
    "actionbutton": true,
    "tooltip": false,
    "viewin": "tooltip"
}
```
Useing [Japanese morphological analysis](https://ja.wikipedia.org/wiki/%E5%BD%A2%E6%85%8B%E7%B4%A0%E8%A7%A3%E6%9E%90) engine [MeCab](http://taku910.github.io/mecab/ "MeCab: Yet Another Part-of-Speech and Morphological Analyzer"), display selected Japanese sentences in morpheme. Since MeCab receives input from standard input instead of command line, we pass the selected string to `"strem"`.

#### `"modify_page"`
Describe the contents of changes made to the current page when the value of [`"action"`](#action) is *`"modify_page"`*. Value is an array of objects. Object keywords are `"selector"`, `"action"`, `"elements"`.

* `"selector"`: Specify the element to be changed with a valid selector such as class, id, element name of [CSS](https://www.google.com/search?q=Cascading+Style+Sheets). String.
* `"action"`: Specify the processing to be performed on the target elements. String. *`prepend`*: add the child elements to the beginning of the target element, *`append`*: add the child elements to the end of the target element, *`remove`*: delete the target elements.
* `"elements"`: Describe the HTML elements to be added. String.

Changes are made to the current page in the order described in the array.

Example:
```json
{
    "name": "Google translate element",
    "contextmenu": ["tab", "page"],
    "action": "modify_page",
    "modify_page": [
        {"selector": "body", "action": "prepend",
         "elements": "<div style='display:none;' id='google_translate_element'></div>"},
        {"selector": "body", "action": "append",
         "elements": "<script type='text/javascript'>function googleTranslateElementInit() {new google.translate.TranslateElement({pageLanguage: '', includedLanguages: 'ja,en'}, 'google_translate_element');}</script><script src='https://translate.google.com/translate_a/element.js?cb=googleTranslateElementInit'></script>"}
    ]
}
```
Add a Google website translation tool that allows you to translate pages like the Google Chrome page translation feature to the current page. Reference: [How To Google Translate](https://www.w3schools.com/howto/howto_google_translate.asp "How To Google Translate")。

#### `"tooltip_width"`
Specify the width of the tooltip panel as the number of pixels when the value of [`"viewin"`](#viewin) is *`"tooltip:`*. integer. The default is the value specified on option page.

#### `"tooltip_height"`
Specifies the height of the tooltip panel as the number of pixels when the value of [`"viewin"`](#viewin)  is *`"tooltip"`*. integer. The default is the value specified on option page.

#### `"tooltip_maximized"`
When the value of [`"viewin"`](#viewin) is *`"tooltip"`*, specify whether to maximize tooltip panel and display it. Boolean. Default is *`false`*, do not maximize.

#### `"panel_width"`
Specify the width of the browser panel in pixels when the value of [`"viewin"`](#viewin) is *`"panel"`*. integer. The default is the value specified on option page.

#### `"panel_height"`
Specifies the height of the browser panel in pixels when the value of [`"viewin"`](#viewin) is *`"panel"`*. integer. The default is the value specified on option page.

#### `"baction_panel_width"`
When the value of [`"viewin"`](#viewin) is *`"browser_action"`*, specify the width of the search result display area in the browser action in pixels. Integer. The default is the value specified on option page.

#### `"baction_panel_height"`
When the value of [`"viewin"`](#viewin) is *`"browser_action"`*, specify the height of the search result display area in the browser action in pixels. Integer. The default is the value specified on option page.

#### `"close_panel_on_inactivated"`
Specify whether to automatically delete the browser panel when a tab in another window is clicked. Boolean. If the value is `true`, erase the browser panel when the tab of another window is selected. When the value is `false`, nothing is done. It works when the tab of another window is clicked instead of when it is focused. It does not work when clicking on a tab displaying a special page such as `about:*` or [`addonns.mozilla.org`](https://addons.mozilla.org/firefox/firefox/"Add-ons for Firefox"). If `"close_panel_on_inactivated"` is not described, follow option page's specification.

#### `"close_tab_on_inactivated"`
Specify whether to automatically delete the tab of the search result when another tab is activated. Valid when the value of [`"viewin"`](#viewin) is *`"other_tab"`* or the value of [`"viewin"`](#viewin) is *`"tab"`* and to display on another tab is selected on the options page. Boolean. If the value is `true`, delete the tab of the search result when another tab is selected. It is not deleted when the value is `false`. If `"close_tab_on_inactivated"` is not described, follow the option page's specification.

#### `"search_when_mouseover"`
Whether to initiate a query simply by hovering over the small icon displayed when selecting a string. Boolean. Default is *` false`*, and the inquiry is started when the icon is clicked with the mouse.

#### `"button_border_color"`
Specify the color of the icon frame of the tool tip search button. String. When the same icon is used for multiple services, change the color of the border to make it easier to distinguish individual services. For example, separate the colors of the buttons for translating English to Japanese and translating from Japanese to English in Google Translate. The method of designating the color is the same as [color](https://developer.mozilla.org/docs/Web/CSS/color_value "<color> - CSS | MDN") of [CSS](https://www.google.com/search?q=Cascading+Style+Sheets).

Examples:
```json:services.json
{
    "button_border_color": "red",
    "button_border_color": "#ffa500",
    "button_border_color": "rgb(255, 0, 153)",
```

#### `"icon_name"`
Name of the service icon. Icon image data is described in the [`"icons"`](#icons) section. String.

### "icons"
In `"icons"`, describe the image data of the service icon in the format of *`"name"`*`: `*`"icon image data"`*.

*`"name"`* is the name you specified for [`"icon_name"`](#icon_name) in the [`"services"`](#services) section.

Example:

```json:services.json
{
    "services": [
        {
            "name": "Weblio SEL/CB -> TT",
            "icon_name": "weblio_ejje"
        },
        {
            "name": "selection -> clipboard",
            "icon_name": "clipboard"
        }
    ],
    "icons": {
        "weblio_ejje": "data:image/png;base64,....",
        "clipboard": "data:image/gif;base64,....",
    }
}
```
In *`"icon image data"`*, image data is described in [Base64](https://en.wikipedia.org/wiki/Base64 "Base64 - Wikipedia") format. Icons are displayed in 16x16 dots, so 16x16 dots are good for images, but Firefox will resize even if the sizes are different, so there is no problem. If you use a large image, the image will not be blurred even when you zoom web pages.

It is convenient to use the desired site's [favicon](https://www.google.com/search?q=favicon) for images. 

If the target site offers favicon, you can obtain it as follows. Open a page of the target site and click `View Page Info (I)` on the context menu. Open the `Media (M)` tab of the `Page Info` window. Select an element whose type is `Icon`. If there is more than one, choose one with `Dimension` 16px x 16px. Click `Save As...` to save it.

Or you can search favicon for famous sites on Google. By accessing the http://www.google.com/s2/favicons?domain=_URL_of_the_site_, you get favicon for that site. Example: [`https://www.google.com/s2/favicons?domain=wikipedia.org`](https://www.google.com/s2/favicons?domain=wikipedia.org "favicons (PNG image , 16 x 16 px)")

<form action="http://www.google.com/s2/favicons" method="get" target="_blank">
    <label>Get a site's 16x16 favicon on Google:
        <input type="text" name="domain" required="required" placeholder="domain name">
    </label>
    <input type="submit">
</form>
<form action="http://www.google.com/s2/favicons" method="get" target="_blank">
    <input type="hidden" name="sz" value="32">
    <label>Get a site's 32x32 favicon on Google:
        <input type="text" name="domain" required="required" placeholder="domain name">
    </label>
    <input type="submit">
</form>
<form action="http://www.google.com/s2/favicons" method="get" target="_blank">
    <input type="hidden" name="sz" value="64">
    <label>Get a site's 64x64 favicon on Google:
        <input type="text" name="domain" required="required" placeholder="domain name">
    </label>
    <input type="submit">
</form>

It is necessary to encode image data to Base64 format.

<label>Convert image files to base64 format:
    <input type="file" id="file" accept="image/*" required>
</label>
<img id="image" src="">
<style>
    .dragover {background: #afc7fc;}
</style>
<textarea id="base64_result" cols="80" readonly placeholder="Drop an image file here"></textarea>
<button id="base64_copy">Copy to clipboard</button>
<script src="../base64.js"></script>

## Notes
Unknown keywords are ignored. You can use it like a comment by using this. Example: `"#tooltip_height ": 425`. Conversely, even if you make a typo in the keyword, it does not cause an error. Since it is not a real comment, it is all loaded in the browser and consumes a small amount of memory, so it is better not describe unnecessary items. However, keywords starting with "`//`" are treated as comments and do not consume memory. Example: `"//tooltip_height ": 425`.

Note the comma `", "` separating JSON elements. Commas are not terminators, but delimiters, so adding a comma after the last element results in an error.

Specifications may be changed without considering compatibility with the past. It may become unusable suddenly as the version of the add-on rises. In that case, it is necessary to correct the setting file or return to the old version.

# Setting for using subprocess

## Downloads
* <a href="../yadatta_app.json" download="">App manifest (yadatta_app.json)</a>
* <a href="../yadatta_app.py" download="">native application (yadatta_app.py)</a>
* <a href="../yadatta_app.bat" download="">Windows batch file (yadatta_app.bat)</a>
* <a href="../yadatta_app.reg" download="">Windows registry file (yadatta_app.reg)</a>
* <a href="../yadatta_app_global.reg" download="">Windows registry file (yadatta_app_global.reg)</a>

## Windows
1. If [Python](https://en.wikipedia.org/wiki/Python_(programming_language) "Python (programming language) - Wikipedia") is not installed, download it from [Python's web site](https://www.python.org/) and install it, or install the Python package from [Cygwin](https://www.cygwin.com/">).

1. Download all the above four files and place them anywhere. Example: `C:\Users\user1\Documents\`

1. Edit `yadatta_app.reg` and set the path to point to `yadatta_app.json`.

    Example:

    ```registry
    @="C:\\Users\\user1\\Documents\\yadatta_app.json"
    ```

    Double click on `yadatta_app.reg` in Explorer to start up and register it in the registry.

    > You can register in the registry in the same way by executing as follows at the command prompt.
    > ```cmd
    > reg add "HKEY_CURRENT_USER\SOFTWARE\Mozilla\NativeMessagingHosts\yadatta_app" /ve /d "C:\Users\user1\Documents\yadatta_app.json" /f
    > ```
    > To delete the registry at uninstallation, execute as follows in the command prompt.
    > ```registry
    > reg delete "HKEY_CURRENT_USER\SOFTWARE\Mozilla\NativeMessagingHosts\yadatta_app" /f
    > ```

    If you want to make it available to all users of the computer, download the fifth file `yadatta_app_global.reg` instead of `yadatta_app.reg` and do the same work.

1. Edit `yadatta_app.json` and modify the *`path`* to point to `yadatta_app.bat`.

    Example:

    ```
    "path": "C:\\Users\\user1\\Documents\\yadatta_app.bat",
    ```

1. Edit `yadatta_app.bat` and set python.exe's path.

    Example:

    * When installing from Python's web site

        ```bat
        %USERPROFILE%\AppData\Local\Programs\Python\Python36-32\python.exe -u yadatta_app.py
        ```

    * When using the Cygwin's Python package

       ```bat
       C:\cygwin\bin\python3.6m.exe -u yadatta_app.py
       ```

    Windows batch does not understand Cygwin's symbolic link, so it does not work with `C:\cygwin\bin\python`.

    In either case, if the version of Python goes up, you need to modify the path.

    On Windows 10 and later, you might also be able to use Python by installing [Windows Subsystem for Linux](https://www.google.com/search?q=Windows+Subsystem+for+Linux "Windows Subsystem for Linux - Google Search"). (unconfirmed)

1. Register the service and try to run notepad.exe.

    ```json:services.json
    {
        "services": [
            {
                "name": "notepad",
                "childprocess": {
                    "args": ["C:\\Windows\\System32\\notepad.exe"]
                },
                "contextmenu": ["page", "tab"],
                "action": "childprocess"
            }
        ]
    }
    ```

    Open [Browser Console](https://developer.mozilla.org/en-US/docs/Tools/Browser_Console "Browser Console - Firefox Developer Tools | MDN") and check for errors.

## Linux
1. Install [Python](https://www.python.org/) if it is not installed.

1. Download the first two and place it in `~/.mozilla/native-messaging-hosts/`.

1. Make `yadatta_app.py` executable.

    Example:

    ```shell
    chmod +x yadatta_app.py
    ```

1. Edit `yadatta_app.json` and modify the *`path`* to point to `yadatta_app.py`.

    Example:

    ```
    "path": "/home/user1/.mozilla/native-messaging-hosts/yadatta_app.py",
    ```

1. Register the service and test it.

    ```json:services.json
    {
        "services": [
            {
                "name": "childprocess test",
                "childprocess": {
                    "args": "echo 'Hello World.' > /tmp/test.log"
                },
                "contextmenu": ["page", "tab"],
                "action": "childprocess"
            }
        ]
    }
    ```

    Open [Browser Console](https://developer.mozilla.org/en-US/docs/Tools/Browser_Console "Browser Console - Firefox Developer Tools | MDN") and check for errors.

## Mac OS X
I don't have. Buy me one.

## Android
I want it. Buy me one.

## Reference
* [Native messaging](https://developer.mozilla.org/Add-ons/WebExtensions/Native_messaging)
* [Native Messaging - Google Chrome](https://developer.chrome.com/apps/nativeMessaging)

# Why this add-on was created
There was a very easy-to-use Add-on called [Dictionary Tooltip](https://addons.mozilla.org/en/firefox/addon/dictionary-tooltip), but it was not updated for a long time and it did not work well before I knew it. I searched for similar Add-on, but I could not find a good add-on.

* Many add-ons have fixed dictionary sites, so I can not register dictionary sites I want to use.
* Many add-ons can register a dictionary site but only one or two.
* Many add-ons are dictionaries on the net, and can not use local dictionaries.
* Many add-ons are different from what the user interface like the procedure for drawing a dictionary and how to display is desired.

And so on. I made this add-on because I [嫌だった](https://easypronunciation.com/en/japanese-kanji-to-romaji-converter?initial_text=%E5%AB%8C%E3%81%A0%E3%81%A3%E3%81%9F&Convert_to_japanese=romaji&style=slash#result) that my dictionary lookup was inconvenient. 
