# PowerSync Vite bundling test

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

