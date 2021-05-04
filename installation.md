---
layout: api
title: installation
---

# Installation
{: class="api_key"}

`drawsocket` can be run either as a [standalone server](#standalone-udp-server), or inside the [Max](https://cycling74.com/) media programming application.

# Max Package
{: class="api_key"}

Requires [Max](https://cycling74.com/) version >= 8.1.0, and [CNMAT's Odot library](https://github.com/CNMAT/CNMAT-odot/releases), and works well in the [MaxScore](http://www.computermusicnotation.com) notation framework.

__To install:__
0. Download the latest Max Package release from the [drawsocket-max](https://github.com/drawsocket/drawsocket-max) repository.
1. Unzip the downloaded release folder.
2. Place the `drawsocket` repository in the `/Documents/Max 8/Packages` folder.
3. Restart Max.
4. Make a new patcher and put a `drawsocket` object (abstraction) in the patcher, and go to the `drawsocket` help patch.
5. When running the `drawsocket` server for the first time: click on the `script npm install` message to download the `drawsocket` module and dependency libraries (note that you will need to be connected to the internet for the download to work).
6. Refer to the examples in the `drawsocket` help file, and in the Max Extras menu.
7. See the [Overview](overview.html) and [API](api.html) pages for more details.

# Pure Data
{: class="api_key"}

Using the standalone UDP server, the [odot](https://github.com/CNMAT/CNMAT-odot/releases/tag/1.3.0-rc.3) Pd library you can send `drawsocket` compatible OSC bundles. See also the [pd-communicate.pd](https://github.com/drawsocket/drawsocket/blob/main/example/pd-communication.pd) patch in the `drawsocket/example` folder.

# Standalone UDP Server
{: class="api_key"}

1. Install [node.js](https://nodejs.org/en/) if not already installed.
2. Create a new directory and run: `npm install drawsocket`
3. The `drawsocket` server can then be run by importing the module, initializing the server with `drawsocket.init()`, and starting with `drawsocket.start()`. 
4. Initializing the server with the option `enable_udp` set to `true` starts the HTTP server with an additional UDP server which will listen for incoming `drawsocket` format OSC bundles and route them to the client browsers via WebSockets.
5. See the [Overview](overview.html) and [API](api.html) pages for more details.

Example script:

```
const drawsocket = require('drawsocket');

drawsocket.init({
    node_path: __dirname + '/node_modules/',
    userpath: __dirname,
    http_port: 3004,
    enable_udp: true,
    udp_listen_port: 9999,
    udp_send_port: 7777
});

drawsocket.start();

```


# Server Options
{: class="api_key"}

```
let params = {
    node_path: __dirname + '/node_modules/',    // path to node modules for client includes
    userpath: null,                             // path for custom asset serving folder
    htmltemplate: '/lib/drawsocket-page.html',  // sets main drawsocket page, /lib/drawsocket-page.html is the default
    infopage: "/lib/drawsocket-info.html",      // sets landing page, /lib/drawsocket-info.html is the default
    http_port: 3002,                            // sets HTTP port, 3002 by default
    post: "default",                            // by default, console.log, otherwise settable to other print function (e.g. Max.post)
    outlet: "default",                          // by default, if UDP is enabled outlet will be set to use udp_server.send, else disabled
    enable_udp: false,                          // enable/disable UDP server, off by default
    udp_listen_port: 9999,                      // UDP listen port
    udp_send_port: 7777,                        // UDP send-to port
    udp_send_ip: "127.0.0.1"                    // UDP send-to IP address
}
```

# Test Server
{: class="api_key"}

A test HTTP/UDP server can be run from within the `node_modules/drawsocket` folder, by running `node test`.

Optional arguments set UDP send-to IP and port, for example: 

```
node test --ip=192.168.0.1 --port=4444
```

For testing `drawsocket` from the repository, use the `-d` flag to set the path to `node_modules`:

```
node test --ip=192.168.0.1 --port=4444 -d 
```