Swift Browser
=============

[![Build Status](https://travis-ci.org/zerovm/swift-browser.svg?branch=master)](https://travis-ci.org/zerovm/swift-browser)
[![Coverage Status](https://img.shields.io/coveralls/zerovm/swift-browser.svg)](https://coveralls.io/r/zerovm/swift-browser)

JavaScript based UI for [OpenStack Swift][].

Deployment
==========

Deployment is simple: if you got this file from a release tarball or
zip file, then you simply need to upload all files in this folder to
Swift. That will look like this:

    $ swift post swift-browser
    $ swift post -r '.r:*' swift-browser
    $ swift upload swift-browser .

Then load the `index.html` page in your browser. You will be asked for
your credentials if needed. After logging in, you will see a container
listing. Deployment is now done.

If you cloned the repository you need to install dependencies before
you can deploy Swift Browser. These are managed using [Bower][]. You
can run Bower via [npm][]:

    $ npm install

This will install Bower and other tools needed in the `node_modules`
folder (everything is installed locally). It will then run `bower
install` for you to download AngularJS and other libraries needed.

When the command is done, the `app/` folder will be ready for upload.
Simply upload it to your Swift installation and load the `index.html`
page in your browser (use the instructions above).


Configuration
=============

You can add a configuration file named `config.json` to configure the
authentication type and URL endpoint. The default is to authenticate
using [LiteAuth][]:

    {
        "auth": {
            "type": "liteauth",
            "url": "/auth/v1.0"
        }
    }

For authentication against Keystone, you should configure Swift
Browser like this:

    {
        "auth": {
            "type": "keystone",
            "url": "http://localhost:5000/v2.0/tokens"
        }
    }

Same-Origin Restrictions
========================

For Swift Browser to do Keystone authentication, you will need to make
sure that the browser can send AJAX requests to Keystone. Typically,
Swift will be running on one port number with Keystone running on a
different port number. The same-origin restrictions in browsers forbid
JavaScript from making AJAX calls between different hosts.

So to make Swift Browser talk to Keystone, you need to either

* Make the two servers run on the same origin as seen from the
  browser.

  This can be done by installing a proxy infront of both Keystone and
  Swift. The proxy can then expose Swift under
  `http://example.net/swift` and Keystone under
  `http://example.net/keystone`. This will allow JavaScript code
  loaded from `/swift` to send AJAX requests to `/keystone` without
  problem.

* Install and enable a [CORS (cross-origin resource sharing)][cors]
  middleware for Keystone.

  CORS is a standard for allowing scripts on one origin domain access
  resources in another origin. You basically configure Keystone to
  allow API requests from the origin where Swift is running (or from
  any origin). Please see this [blog post][swift-cors] for details.

Supported Browsers
==================

We support IE 10+ and test with Firefox and Chrome.


Release History
===============

Next Version
------------

* [#157][]: concatenate and minify JavaScript and CSS files. The index
  page is now loaded with 7 requests (down from 23 requests).

0.1.0: 2014-11-06
-----------------

First release with a basic feature set:

* Keystone and LiteAuth authentication.
* Can browse containers and pseudo-directories.
* Can create and delete containers.
* Can edit object content and metadata.
* Can upload and delete objects.


Other Swift File Managers
=========================

* [Swift Explorer][]: implemented in Java.

* [django-swiftbrowser][]: implemented in Python using Django.

* [swift-ui][]: implemented in JavaScript using Backbone.js

[OpenStack Swift]: http://docs.openstack.org/developer/swift/
[Bower]: http://bower.io/
[npm]: https://www.npmjs.org/
[LiteAuth]: https://github.com/zerovm/liteauth
[cors]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS
[swift-cors]: http://blog.yunak.eu/2013/07/24/keystone_cors/
[Swift Explorer]: http://www.619.io/swift-explorer
[django-swiftbrowser]: https://github.com/cschwede/django-swiftbrowser
[swift-ui]: https://github.com/fanatic/swift-ui

[#157]: https://github.com/zerovm/swift-browser/issues/157
