---
layout: api
---

# Use in production

Note: `drawsocket` has been used in large scale production, but as with all technology, **the system must be tested before use in production**. Users are advised to thoroughly test your production network and display devices before any live performance.


# Basic Usage in Max

See the `drawsocket.maxhelp` file for examples.

1. Start the server by sending the `script start` message.
2. On successful startup, an IP address and port number will be printed to the Max console, and the same information will be sent out of the right-most outlet of the abstraction.
3. Open a browser and type in the IP address and port specified in the Max patch, followed by a URL of your choosing. This address will be how you address the client browser from the Max server patch. 
    * For example, if the IP:Port is `192.168.1.1:3002`, and you wanted to use the address `/foo` for your OSC style messaging to the browser, you could type the following address into your browser: `192.168.1.1:3002/foo`. Note that if you are testing on the same computer, you can also use `localhost` instead of the IP address.

For usage with [MaxScore](http://www.computermusicnotation.com), please see `Drawsocket` folder in the Max Extras menu, example number 8, `render2browser.maxscore`.

# Assets
{: class="api_key"}

If you wish to serve file assets to your client browsers (e.g. images, pdfs, sound files, html files, etc.), the files must be in a known folder to the server, commonly referred to as a root "public" folder. The public folder path is set relative to the location of the Max patch containing the `drawsocket` abstraction, and therefore you need to save your patch before any assets can be found (so that the patch has a folder location).

By default, the public folder is set to be the same folder that contains the Max patch. 
```
   someFolder
   ├── yourPatch.maxpatch
   ├── image.png
   └── score.pdf

```


Alternatively, to keep the folders a little neater, the `drawsocket` abstraction can be initialized with an argument of the folder path relative to the Max patch location. For example, if you initialize `drawsocket` with the relative path `public`, it will expect the folder `public` to be at the same folder level:
```
   someFolder
   ├── yourPatch.maxpatch
   └── public
      ├── image.png
      └── score.pdf

```

# Default HTML and CSS files
{: class="api_key"}


By default the `drawsocket` server responds to all URL requests with the template HTML page, `drawsocket-page.html` which loads the required Javascript files, sets up the basic HTML objects, layers, and imports the `drawsocket-default.css` which sets up some default display properties.

If desired, a different template HTML page may be used by sending the `drawsocket` object the `html_template` message followed by a relative path to the template HTML file to use.

# Message Format
{: class="api_key"}

All messages in the `drawsocket` API are formatted as an object, enclosed by curly braces `{ }`. Messages can be encoded as Odot nested sub-bundles, or nested JSON objects. In the examples below we will be using OSC (odot) formatting, however you may also use a Max Dictionary, which will be exactly the same, except that address names will not have the leading `/` character.

Odot:
```
{
   /bundle : {
       /subbundle : {
           /foo : 1
       }
   }
}
```
JSON (Max Dictionary):
```
{
   "bundle" : {
       "subbundle" : {
           "foo" : 1
       }
   }
}
```

## Addressing the Client Browser
{: class="api_key"}

The URL used by the client to log into the server IP address and port is used by the Max patch as an OSC address to route messages to all clients logged into a given URL. For example, any users logged into the example above, `192.168.1.1:3002/foo`, will receive OSC messages with the address `/foo`.

## `key` and `val`
{: class="api_key"}

Messages to the client are formatted as objects with `key` and `val` addresses
* The `key` value is a switch key which tells the client how it should interpret the messages in the `val` field. For example, valid `key` values include `svg`, `html`, `tween`, and so on. See below for more details on these options.
* The `val` value, stores one or more objects to be handled by the client. 
 
For example, with a `svg` key, the `val` object might create a new SVG object. In this example, we ask all clients logged into `/foo` to create a new SVG `rect`, using the drawsocket-SVG `new` keyword:

``` 
/foo : {
    /key : "svg",
    /val : {
        /new : "rect",
        /id : "rectangular",
        /x : 100,
        /y : 100,
        /width : 25,
        /height : 25
    }
}
```

The wildcard `*` will match all URL clients, so for example if you replace `/foo` above with the address `/*` the above example would be sent to all clients.

## Unique ID Reference
{: class="api_key"}

Each drawn object needs to have a unique name to identify the object. The name can be any combination of numbers and letters, but needs to be unique. This id can be used to identify the object in situations where you want to change the color, position or other attributes.

For example, in the above example, we we set the `id` to be the name "rectangular". If we have already created the object (in this case using the drawsocket-SVG `new` keyword), we can alter attributes of the rectangle, by referring to the `id`. Here we change the width of the rectangle:

``` 
/foo : {
    /key : "svg",
    /val : {
        /id : "rectangular",
        /width : 50
    }
}
```


# Messages to Drawsocket
{: class="api_key"}


## Storing the Sever State
{: class="api_key"}

The `drawsocket` object in Max accepts the `writecache` message, to write the current cached messages to a file on disk.

The folder path is relative to the folder path of the patch in which the `drawsocket` object is in.

Message syntax:

`writecache <relative folder path>/<filename>.json`

or, to write only one URL prefix:

`writecache <relative folder path>/<filename>.json /myURLPrefix`


## Importing Server Cache from File
The `drawsocket` object in Max accepts the `importcache` message, to read a file from disk and import one or all `prefix` objects in the file.

The folder path is relative to the folder path of the patch in which the `drawsocket` object is in.

Message syntax:

`importcache <relative folder path>/<filename>.json`

or, to read only one URL prefix:

`importcache <relative folder path>/<filename>.json /myURLPrefix`


## Using stored JSON files on other servers
A stored server/client state, saved in JSON format, may also be for online viewing, without the realtime WebSocket system, by serving the `drawsocket-default.html` file (with the associated scripts, and CSS files), and specifying a file name and prefix to load as discussed above via the `file` key.

For example, on a website called `www.foo.com` and a stored JSON file named `stored-cache.json`, we could load the `/1` OSC-URL prefix by using the following URL arguments (using the standard `?`,`&`, `=` special characters):

`http://www.foo.com/drawsocket-default.html?fetch=stored-cache.json&prefix=/1`

(Of course you could also save the HTML file under a different name of your choosing for your server)

# ping
{: class="api_key"}

The `drawsocket` object accepts the `ping` Max message to query the connection status of one or more clients.
For example, the message `ping /*` pings all clients.

# statereq
{: class="api_key"}

The `drawsocket` object accepts the `statereq` Max message to trigger a client update request for one or more clients.

For example, the message `statereq /*` triggers a state request for all clients.

# port
{: class="api_key"}

The `drawsocket` object accepts the `port` Max message to set the server port number. Takes effect on start up.

# html_root
{: class="api_key"}

The `drawsocket` object accepts the `html_root` Max message to add a public asset folder to the server search path. Takes effect on start up.


