# PowerSync Vite bundling test

vite 开发环境下会出现如下问题：
Uncaught SyntaxError: The requested module '/@fs/private/tmp/example-vite/node_modules/can-ndjson-stream/can-ndjson-stream.js?v=1a11c5f8' does not provide an export named 'default' (at AbstractStreamingSyncImplementation.js?v=1a11c5f8:4:8)


## npm run dev
npm works by using optimizeDeps.include
```
  optimizeDeps: {
    // Don't optimize these packages as they contain web workers and WASM files.
    // https://github.com/vitejs/vite/issues/11672#issuecomment-1415820673
    exclude: ["@journeyapps/wa-sqlite", "@journeyapps/powersync-sdk-web"],
    include: [
      "uuid",
      "js-logger",
      "can-ndjson-stream",
      "lodash/throttle",
      "event-iterator"
    ],
  },
```


```
npm install
npm run dev
```

## yarn dev

yarn 与 npm 的行为是一样的

```
yarn install
yarn dev
```