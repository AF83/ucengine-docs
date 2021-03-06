# File Upload

Upload file in current meeting, list uploaded files and allow user to preview or to share one of this files.

## Location

https://github.com/AF83/ucengine-widgets

## Dependencies

* [[jQuery UI|http://jqueryui.com/]]
* [[UCE widget|https://github.com/AF83/ucengine/blob/master/wwwroot/lib/ucewidget.js]]
* [jQuery strftime](http://projects.nocternity.net/index.py/en/jquery-strftime)

## Options

Option         | Description
---------------|---------------------------------------------------------------
ucemeeting     | The current meeting
uceclient      | The U.C.Engine client
upload         | Show or hide the upload button
title          | The title to display in the widget header

## Methods

Method         | Description
---------------|---------------------------------------------------------------
startPreview   | Hide the list of upload files and show a preview screen for the chosen file
stopPreview    | Hide the current preview and show the list of available files
clear          | Reinitialize the widget
destroy        | Destroy the widget

## Javascript Events

Name         | Description
-------------|---------------------------------------------------------------
updated      | called when a new message is received

## Examples

```javascript
$("#placeholder").file_upload({ucemeeting: meeting, upload: true});
```

## Events
### document.share.start

Sent by a user. Notifies other participants that a new file is shared.

Metadata       | Description
---------------|-------------------------------------------------------------------------------------------------------
id             | The document id to share
**optional**   |
page           | The starting page number

Example:

```javascript
{ "type"     : "document.share.start",
  "domain"   : "localhost",
  "datetime" : 1292601125242,
  "id"       : "91918360913598370296768635184385",
  "location" : "my_meeting",
  "from"     : "uid_63444326443_50150",
  "metadata" : {
    "id"   : "file_id_43243253253.pdf",
    "page" : "3",
  }
}
```

## Access rights

Action  | Object        | Conditions    | Description
--------|---------------|---------------|------------------------------------------------------------------------------------------------
add     | file          |               | Allow the user to add a file. If true, display the upload button.
delete  | file          | id            | Allow the user to delete the file with the identifier 'id'. If true, display a 'delete' button.
delete  | file          |               | Delete all files. If true, display 'delete' buttons for every files.
share   | document      | id            | Share a document with the file identifier 'id'. If true, add a 'share' link.
share   | document      |               | Share all documents. If true, add 'share' links for every files.

## Theming

```html
<article id="fileupload" class="ui-widget ui-fileupload">
    <div class="ui-widget-header ui-corner-all ui-helper-clearfix">
        <span class="ui-widget-header-left">
            <span class="ui-toolbar-button ui-button-fold ui-button ui-widget ui-state-default ui-corner-all ui-button-icon-only" role="button" aria-disabled="false" title="">
                <span class="ui-button-icon-primary ui-icon ui-icon-triangle-1-s"></span>
                <span class="ui-button-text"></span>
            </span>
        </span>
        <span class="ui-widget-header-title">Files</span>
        <span class="ui-widget-header-right">
            <span class="ui-toolbar-button ui-button-toggle ui-button ui-widget ui-state-default ui-corner-all ui-button-icon-only" role="button" aria-disabled="false" title="">
                <span class="ui-button-icon-primary ui-icon ui-icon-circle-plus"></span>
                <span class="ui-button-text"></span>
            </span>
        </span>
    </div>

    <div class="ui-widget-content">
        <div class="ui-fileupload-files">
            <ul class="ui-fileupload-list">
                <li class="mime pdf">
                    <p><span class="ui-fileupload ui-filesharing-filename">production-programming.pdf</span></p>
                    <p><span class="ui-file-owner"> 02-21-11 by root</span></p>
                    <p><a href="#" class="ui-fileupload ui-download-link">Download</a></p>
                </li>
            </ul>
            <div>
                <p class="ui-fileupload-add">
                    <a href="#" class="ui-button ui-widget ui-state-default ui-corner-all ui-button-text-only" role="button" aria-disabled="false">
                        <span class="ui-button-text">Upload New File</span>
                     </a>
                </p>
            </div>
        </div>
        <div class="ui-fileupload-preview">
        <div class="ui-corner-all ui-preview-toolbar">
            <span class="ui-fileupload ui-toolbar-button ui-button-previous ui-button ui-widget ui-state-default ui-corner-all ui-button-icon-only" href="#" role="button" aria-disabled="false" title="">
                <span class="ui-button-icon-primary ui-icon ui-icon-arrowthick-1-n"></span>
                <span class="ui-button-text"></span>
            </span>
            <span class="ui-fileupload ui-toolbar-button ui-button-next ui-button ui-widget ui-state-default ui-corner-all ui-button-icon-only" href="#" role="button" aria-disabled="false" title="">
                <span class="ui-button-icon-primary ui-icon ui-icon-arrowthick-1-s"></span>
                <span class="ui-button-text"></span></span>
                <span class="ui-fileupload ui-toolbar-button ui-button-stop ui-button ui-widget ui-state-default ui-corner-all ui-button-icon-only" href="#" role="button" aria-disabled="false" title="">
                <span class="ui-button-icon-primary ui-icon ui-icon-circle-close"></span>
                <span class="ui-button-text"></span>
            </span>
            <span class="ui-fileupload ui-toolbar-selector">
                <span class="ui-fileupload ui-selector-current"></span>
                <span class="ui-fileupload ui-selector-separator">/</span>
                <span class="ui-fileupload ui-selector-total"></span>
            </span>
        </div>
        <div class="ui-fileupload-page">
            <img>
        </div>
    </div>
</article>
```
