# Deno `fs.existsSync` bug

The implemnetation for `fs.existsSync` unexpectedly throws when a path does not exist.

## Steps to reproduce:

1. Clone this repository
2. Run `deno task dev`

Error output:

```sh
error: Uncaught NotFound: No such file or directory (os error 2): lstat '/project/node_modules/.vite'
    at Object.lstatSync (ext:deno_fs/30_fs.js:339:7)
    at Object.existsSync (ext:deno_node/_fs/_fs_exists.ts:28:14)
    at cleanupDepsCacheStaleDirs (file:///project/node_modules/.deno/vite@4.3.4/node_modules/vite/dist/node/chunks/dep-f7d05e3f.js:44801:18)
    at Timeout._onTimeout (file:///project/node_modules/.deno/vite@4.3.4/node_modules/vite/dist/node/chunks/dep-f7d05e3f.js:44103:26)
    at cb (ext:deno_node/internal/timers.mjs:37:46)
    at Object.action (ext:deno_web/02_timers.js:149:11)
    at handleTimerMacrotask (ext:deno_web/02_timers.js:66:10)
    at eventLoopTick (ext:core/01_core.js:187:21)
```

The error originates here https://github.com/vitejs/vite/blob/c63ba3fa08a64d75bfffa6885dc4c44875b9c5ba/packages/vite/src/node/optimizer/index.ts#L1335 .
