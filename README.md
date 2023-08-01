# Manual npm script deploy over ssh

No downtime while React app builds on the server.

Bypass "500 Internal Server Error" screen message.

## Prerequisites

Install npm package copyfiles as a dev dependency. https://www.npmjs.com/package/copyfiles

<code>npm -i copyfiles --save-dev</code>

## Process

In package.json add 2 new scripts:

<code>"prebuild": "BUILD_PATH='./prebuild' react-scripts build"</code>

This will change Build path to ./prebuild (supported with react-scripts >= 4.0.2).

<code>"copy": "copyfiles -u 1 'prebuild/\*_/_' build && rm -rf prebuild"</code>

This will copy everything from temporary build folder ./prebuild to ./build and remove the temporary folder as soon as the content copying is done.

Change your main "build" script to run the previous added scripts:

<code>"build": "npm run prebuild && npm run copy"</code>

Run <code>npm run build</code>

Until all the content from ./prebuild is copied into ./build, old react app version from ./build folder will remain online.
