这个问题未解决，暂时没法用 pnpm 开发。Problem with Dependency Optimization in pnpm #16293 https://github.com/vitejs/vite/issues/16293



# PowerSync Vite bundling test

原项目：
https://github.com/powersync-ja/powersync-js/tree/main/demos/example-vite


https://vitejs.dev/guide/dep-pre-bundling.html
Dependency Pre-Bundling 的作用：
1. CommonJS 转 ESM | CommonJS and UMD compatibility: During development, Vite's dev serves all code as native ESM. Therefore, Vite must convert dependencies that are shipped as CommonJS or UMD into ESM first.
2. 一堆小文件转成1个文件 | Performance: Vite converts ESM dependencies with many internal modules into a single module to improve subsequent page load performance

出现问题在第1点。为何在使用 pnpm 的时候失效了。
总的来说，是 Vite 处理 CommonJS 包的问题。pnpm 的链接问题导致的么？？？




pnpm 用这个也没效果
  "dependenciesMeta": {
    "uuid": {
      "injected": true
    }
  },

vite 开发环境下会出现如下问题：
Uncaught SyntaxError: The requested module '/@fs/private/tmp/example-vite/node_modules/can-ndjson-stream/can-ndjson-stream.js?v=1a11c5f8' does not provide an export named 'default' (at AbstractStreamingSyncImplementation.js?v=1a11c5f8:4:8)

1. npm/yarn 环境下通过 optimizeDeps.include 解决问题。(需要被链接的依赖被导出为 ESM 格式)
2. pnpm 未解决。vite pre bundling 的作用就是将 commonjs CJS 自动转换位 ESM，但是失败了？

build 都是没有问题的。


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

有时会遇到：
```
Failed to resolve dependency: uuid, present in 'optimizeDeps.include'
Failed to resolve dependency: js-logger, present in 'optimizeDeps.include'
Failed to resolve dependency: can-ndjson-stream, present in 'optimizeDeps.include'
Failed to resolve dependency: lodash/throttle, present in 'optimizeDeps.include'
Failed to resolve dependency: event-iterator, present in 'optimizeDeps.include'
```

https://github.com/vitejs/vite/issues/16293

未解决 https://github.com/vitejs/vite/issues/11783

他遇到的是打包出错，开发正常 https://keqingrong.cn/blog/2021-11-24-commonjs-compatibility-with-vite/