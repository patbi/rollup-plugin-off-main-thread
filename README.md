# rollup-plugin-off-main-thread

Use Rollup with workers and ES6 modules _today_.

```
$ npm install --save rollup-plugin-off-main-thread
```

Workers are JavaScript’s version of threads. [Workers are important to use][when workers] as the main thread is already overloaded, especially on slower or older devices.

This plugin gives Rollup an understanding of `new Worker()` and takes care of shimming module support in workers.

## Usage

```js
// rollup.config.js
import omt from "rollup-plugin-off-main-thread";

export default {
  input: ["src/main.js"],
  output: {
    dir: "dist",
    // You _must_ use “amd” as your format
    format: "amd"
  },
  plugins: [omt()]
};
```

## Options

```js
{
  // ...
  plugins: [omt(options)];
}
```

- `loader`: A string containing the EJS template for the amd loader. If `undefined`, OMT will use `loader.ejs`.
- `useEval`: Use `fetch()` + `eval()` to load dependencies instead of `<script>` tags and `importScripts()`. _This is not CSP compliant, but is required if you want to use dynamic imports in ServiceWorker_.
- `marker`: A string that is temporarily injected to mark `new Worker()` calls. It’s only purpose is to be unique enough that the string sequence can’t appear by coincidence in ohter places. The default should be fine 99% of the time.
- `workerRegexp`: A RegExp to find `new Workers()` calls. The second capture group _must_ capture the provided file name without the quotes.
- `filenameRegexp`: A RegExp that finds the file name after Rollup has transformed dynamic imports.

[when workers]: https://dassur.ma/things/when-workers

---

License Apache-2.0
