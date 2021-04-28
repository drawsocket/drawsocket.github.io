---
layout: page
title: installation
---

# Download

To get started, [download the latest release from the GitHub repository](https://github.com/HfMT-ZM4/drawsocket/releases).

# Installation

`drawsocket` can be run either as a standalone [node.js](https://nodejs.org/en/) server, or inside the [Max](https://cycling74.com/) media programming application.

### Max Package

Requires [Max](https://cycling74.com/) version >= 8.1.0, and [CNMAT's Odot library](https://github.com/CNMAT/CNMAT-odot/releases), and works well in the [MaxScore](http://www.computermusicnotation.com) notation framework.

__To install:__
1. Unzip the downloaded release folder.
2. Place the `drawsocket` repository in the `/Documents/Max 8/Packages` folder.
3. Restart Max.
4. Make a new patcher and put a `drawsocket` object (abstraction) in the patcher, and go to the `drawsocket` help patch.
5. When running the `drawsocket` server for the first time: click on the `script npm install` message to download the required packages and libraries (note that you will need to be connected to the internet for the download to work).
6. Refer to the examples in the `drawsocket` help file, and in the Max Extras menu.
7. See the [Overview](overview.html) and [API](api.html) pages for more details.
   
### Standalone Server

Requires [node.js](https://nodejs.org/en/) to be installed and an internet connection.

__To install:__
1. Unzip the downloaded release folder.
2. Open a terminal window, and navigate to the `drawsocket/code/node` folder.
4. Run `npm install` in the `node` folder, to download the required dependencies.
5. Run `node .` to start the server.
6. By default the server listens to UDP port `9999` and sends messages to localhost port `7777`.
7. See the [Overview](overview.html) and [API](api.html) pages for more details.