# Bug

Railpack will not include Node.js when the start script is set to run with Bun.js, which causes the build process to fail. The "nuxt build" command is a binary file with a Node.js shebang (#!/usr/bin/env node), which Bun.js respects and tries to execute with Node.js, causing the build to break when Node.js is not available.

Workaround:
This can be fixed by forcing Bun.js to execute the Nuxt Binary as a runtime (even if the shebang calls for Node.js) by using the --bun flag in the script as "bun --bun nuxt build"

Railpack should be smart enough the include Node.js if there is a shebang for node in a called bin like nuxt.

Breaks Build process:

```json
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
```

Works:

```json
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
```

