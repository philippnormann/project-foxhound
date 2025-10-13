# Taint Export Extension

This extension automatically exports taint flows discovered by Foxhound to an external server.

## Configuration

To activate the extension, set the following preference (in about:config):

```
tainting.export.url
```

to a String containing the URL where your export server is listening. If an empty string is provided (default), then no request will be sent.

For each taint flow detected by Foxhound, a POST request will be sent to the server containing a JSON-formatted version of the taint flow.

## Developer

The extension injects a content script [taint-export.js](./taint-export.js) into each webpage, which adds an Event listener for the ```__taintreport``` event. When the event is fired, the script checks the preference and sends the taint information via a ```fetch``` request.

The preference containing the URL is accessed via the ```browser.tainting.getTaintExportUrl()``` extension API, which can be found under ```toolkit/components/extensions/ext-toolkit.json```. We need a built-in extension API as the experimental APIs for individual extensions (e.g. screenshots) are not available to content scripts. 

More information on browser extension APIs can be found here: https://firefox-source-docs.mozilla.org/toolkit/components/extensions/webextensions/basics.html