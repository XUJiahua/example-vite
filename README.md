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

## pnpm dev (fixme)

原仓库中只使用 pnpm start = pnpm build && pnpm preview , pnpm dev 都是可以的。
但是原仓库中，即使删除了 optimizeDeps.include 也是可以运行的。

在这个仓库里，在 optimizeDeps.include 中的设置都失效了（npm/yarn 中可用）

```
Failed to resolve dependency: uuid, present in 'optimizeDeps.include'
Failed to resolve dependency: js-logger, present in 'optimizeDeps.include'
Failed to resolve dependency: can-ndjson-stream, present in 'optimizeDeps.include'
Failed to resolve dependency: lodash/throttle, present in 'optimizeDeps.include'
Failed to resolve dependency: event-iterator, present in 'optimizeDeps.include'
```

https://github.com/vitejs/vite/issues/16293