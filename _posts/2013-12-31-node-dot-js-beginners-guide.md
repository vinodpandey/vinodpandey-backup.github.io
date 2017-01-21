---
layout: post
title: "Node.js Beginners Guide"
date: 2013-12-31 10:56
comments: true
categories: 
---
# Overview
Node.js is a javascript based software platform for building fast, scalable network applications. It can be used for data-intensive real-time applications running across distributed devices. 


# Installation
## Installing Node.js on Mac

1. Download [Node.js](http://nodejs.org) and install (double click on node-v0.10.24.pkg and the installer should take care of everything)
2. Verify npm is installed properly

```sh
npm --version
1.3.21
```

## Node.js in action.  

Create example.js with below content. Below code creates a webserver that will respond with 'Hello World' 
  
```javascript
var http = require('http');
http.createServer(function (req, res) {
  res.writeHead(200, {'Content-Type': 'text/plain'});
  res.end('Hello World\n');
}).listen(1337, '127.0.0.1');
console.log('Server running at http://127.0.0.1:1337/');
```

Executing example.js 

```sh
node example.js
Server running at http://127.0.0.1:1337/
```

Access http://127.0.0.1:1337/ and you are greeted with 'Hello World'  

## GoogleTechTalk from Ryan Dahl (Node.js creator)

<iframe width="420" height="315" src="//www.youtube.com/embed/F6k8lTrAE2g?rel=0" frameborder="0"> </iframe>



