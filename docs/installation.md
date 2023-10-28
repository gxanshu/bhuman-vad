# Installation

bhuman-vad comes with a folk of `ricky0123/vad-web` so we can customize it as our own way.
installing this package is really so simple.

<!-- installation section -->

run 
```bash
npm install bhuman-vad-web
```

this will install the package in your project now you just have to configure it.

## Configration

`ricky0123/vad` have alot of problems when it comes to step here is the quick guide you can follow to make thinks easy.

make issue everyone faces is in including `.onnx` & `.wasm` dependecies. so here is the solution

### Bundler

if you are using `webpack` then this method can help you. `bhuman-vad` uses [onnx](https://onnxruntime.ai/) to make things happens under the hood so you have to include there `.onnx` file and `wasm` file in your bundle to run vad properly.

here is the example how you can use it 

```js
// webpack.config.js
const CopyPlugin = require("copy-webpack-plugin")

module.exports = {
  mode: "development",
  plugins: [
    new CopyPlugin({
      patterns: [
        {
          from: "node_modules/bhuman-vad-web/dist/vad.worklet.bundle.min.js",
          to: "[name][ext]",
        },
        {
          from: "node_modules/bhuman-vad-web/dist/*.onnx",
          to: "[name][ext]",
        },
        { from: "node_modules/onnxruntime-web/dist/*.wasm", to: "[name][ext]" },
        { from: "src/index.html", to: "[name][ext]" },
      ],
    }),
  ],
  output: {
    clean: true,
  },
}
```

and you are able to use vad in your project. if there any problem happens try with script method.

### NextJS

One caveat - when using the Nextjs development server, the VAD doesn't behave properly and seems to create two voice activity detectors. I think this may be because `react strict mode` is enabled, but I have not yet checked whether things work when this is disabled. However, when running with `npm run build && npm run start`, the hook appears to work as intended.

in order to include `.onnx` & `.wasm` in nextjs project you can customize `next-config.js`

```js
// next-config.js
const CopyPlugin = require("copy-webpack-plugin")

/** @type {import('next').NextConfig} */
const nextConfig = {
  reactStrictMode: true,

  webpack: (config, {}) => {
    config.resolve.extensions.push(".ts", ".tsx")
    config.resolve.fallback = { fs: false }

    config.plugins.push(
      new CopyPlugin({
        patterns: [
          {
            from: "./node_modules/onnxruntime-web/dist/ort-wasm.wasm",
            to: "static/chunks/[name][ext]",
          },
          {
            from: "./node_modules/onnxruntime-web/dist/ort-wasm-simd.wasm",
            to: "static/chunks/[name][ext]",
          },
          {
            from: "node_modules/bhuman-vad-web/dist/vad.worklet.bundle.min.js",
            to: "static/chunks/[name][ext]",
          },
          {
            from: "node_modules/bhuman-vad-web/dist/*.onnx",
            to: "static/chunks/[name][ext]",
          },
        ],
      })
    )

    return config
  },
}

module.exports = nextConfig
```

### React bundler

same as `Nextjs` we have to include our dependencies in `webpack-config.js` file

```js
const CopyPlugin = require("copy-webpack-plugin")

module.exports = {
  entry: "./src/index.jsx",
  module: {
    rules: [
      {
        test: /\.m?jsx?$/,
        exclude: /node_modules/,
        use: {
          loader: "babel-loader",
          options: {
            presets: [["@babel/preset-react"]],
          },
        },
      },
    ],
  },
  plugins: [
    new CopyPlugin({
      patterns: [
        {
          from: "node_modules/@ricky0123/vad-web/dist/vad.worklet.bundle.min.js",
          to: "[name][ext]",
        },
        {
          from: "node_modules/@ricky0123/vad-web/dist/*.onnx",
          to: "[name][ext]",
        },
        { from: "node_modules/onnxruntime-web/dist/*.wasm", to: "[name][ext]" },
        { from: "src/index.html", to: "[name][ext]" },
      ],
    }),
  ],
  output: {
    clean: true,
  },
}
```

### Script Tags (works 100%)

sometimes there maybe an issue while including these dependencies in your bundler like `solidjs` uses `vite` for bundling and maybe like `svalte` or something.

i still don't know how to incude those dependencies in with `vite` but there is a solution that can work everywhare in browser.

all you have to do is to include scripts in your html file.

if you are using `react`, `svalte`, `solidjs` or any raw framework then you can directly import scripts in your project and use VAD.

only problem with this method is you will unable to TypeScript types definations. but still no problem you can count on documenation.

This example shows how to use the VAD using script tags. Note that onnxruntime-web must also be included in script tags prior to loading the VAD.

inside `index.html`

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Demo of bhuman-vad-web</title>
  </head>
  <body>
    <div class="content-container">
      <!-- your html -->
    </div>
  </body>
  <!-- import script here -->
  <script src="https://cdn.jsdelivr.net/npm/onnxruntime-web/dist/ort.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/bhuman-vad-web@0.0.1/dist/bundle.min.js"></script>

</html>
```

now you can use VAD. if there any issue you encounter while bundling you can try with scripts.

### how to add scripts in NextJS ?

super simple just use `<Script/>` component from Nextjs and import those two scripts and it will work.

also you have more freedom in nextjs to import these scripts only when spacific component mounts.

just use script tag only inside that component where you are using vad and you're good to goo.