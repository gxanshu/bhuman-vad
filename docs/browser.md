# Browser

# User guide for browser use

The `bhuman-vad-web` package aims to provide an accurate, user-friendly voice activity detector (VAD) that runs in the browser.

## Script tags quick start

The VAD can be used via script tags as follows:

```html
<script src="https://cdn.jsdelivr.net/npm/onnxruntime-web/dist/ort.js"></script>
<script src="https://cdn.jsdelivr.net/npm/bhuman-vad-web@0.0.1/dist/bundle.min.js"></script>
<script>
  async function main() {
    const myvad = await vad.MicVAD.new({
      onSpeechEnd: (audio) => {
        // do something with `audio` (Float32Array of audio samples at sample rate 16000)...
      },
    })
    myvad.start()
  }
  main()
</script>
```

## Bundling

To use the VAD in a frontend project managed by a bundler like Webpack, install `bhuman-vad-web` with a command like

```sh
npm i bhuman-vad-web
```

and use an import like:

```js
import { MicVAD } from "bhuman-vad-web"
const myvad = await MicVAD.new({
  onSpeechEnd: (audio) => {
    // do something with `audio` (Float32Array of audio samples at sample rate 16000)...
  },
})
myvad.start()
```

You will also need to

1. serve the `silero_vad.onnx` file that comes distributed with `bhuman-vad-web`
1. serve the `vad.worklet.bundle.min.js` file that comes distributed with `bhuman-vad-web`
1. serve the wasm files that come distributed with the package `onnxruntime-web`

One way to accomplish this is to run a shell script that copies these files into your `dist` directory (or whatever you have named your output directory) during your build process - see the [build script](https://github.com/ricky0123/vad/blob/master/site/scripts/build.sh) for this website for an example. Or, if you are using Webpack 5, this can be acheived by adding the following to your webpack.config.js (other bundlers may have similar options/plugins):

```js
const CopyPlugin = require("copy-webpack-plugin")

module.exports = {
  // ...
  plugins: [
    // ...
    new CopyPlugin({
      patterns: [
        // ...
        {
          from: "node_modules/bhuman-vad-web/dist/vad.worklet.bundle.min.js",
          to: "[name][ext]",
        },
        {
          from: "node_modules/bhuman-vad-web/dist/*.onnx",
          to: "[name][ext]",
        },
        { from: "node_modules/onnxruntime-web/dist/*.wasm", to: "[name][ext]" },
      ],
    }),
  ],
}
```

Note that you will need to install `copy-webpack-plugin` in order for the webpack config snippet above to work (`npm i -D copy-webpack-plugin` if using npm).
