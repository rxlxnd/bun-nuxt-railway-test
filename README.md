# Bug

Railpack will not include Node.js when the start script is set to run with bun, which causes the build process to fail. The "nuxt build" command is a binary file with a Node.js shebang (#!/usr/bin/env node), which bun respects and tries to execute with node, causing the build to break when Node.js is not available.

Workaround:
this an be fixed by setting forcing bun to execute it as a runtime (even if the shebang calls for node) by using the --bun flag in the script  "bun --bun nuxt build"

Railpack should be smart enough the include node if there is a shebang for node in a called bin like nuxt.

Breaks Build process:

{
  "name": "nuxt-app",
  "type": "module",
  "private": true,
  "scripts": {
    "build": "nuxt build",
    "dev": "nuxt dev",
    "generate": "nuxt generate",
    "preview": "nuxt preview",
    "postinstall": "nuxt prepare",
    "start": "bun .output/server/index.mjs" 
  },
  "dependencies": {
    "nuxt": "^4.1.2",
    "vue": "^3.5.21",
    "vue-router": "^4.5.1"
  }
}

Works:

{
  "name": "nuxt-app",
  "type": "module",
  "private": true,
  "scripts": {
    "build": "nuxt build",
    "dev": "nuxt dev",
    "generate": "nuxt generate",
    "preview": "nuxt preview",
    "postinstall": "nuxt prepare",
    "start": "node .output/server/index.mjs"
  },
  "dependencies": {
    "nuxt": "^4.1.2",
    "vue": "^3.5.21",
    "vue-router": "^4.5.1"
  }
}

