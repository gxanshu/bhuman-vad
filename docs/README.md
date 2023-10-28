# BHuman's Voice Activity Detection for Javascript

folk of [@ricky0123](https://github.com/ricky0123/vad)

[![npm vad-web](https://img.shields.io/npm/v/@ricky0123/vad-web?color=blue&label=%40ricky0123%2Fvad-web&style=flat-square)](https://www.npmjs.com/package/@ricky0123/vad-web)
[![npm vad-node](https://img.shields.io/npm/v/@ricky0123/vad-node?color=blue&label=%40ricky0123%2Fvad-node&style=flat-square)](https://www.npmjs.com/package/@ricky0123/vad-node)
[![npm vad-react](https://img.shields.io/npm/v/@ricky0123/vad-react?color=blue&label=%40ricky0123%2Fvad-react&style=flat-square)](https://www.npmjs.com/package/@ricky0123/vad-react)

> :warning: Please use the new platform-specific packages: `@ricky0123/vad-web`, `@ricky0123/vad-node`, etc.

This package aims to provide an accurate, user-friendly voice activity detector (VAD) that runs in the browser. It also has limited support for node. Currently, it runs [Silero VAD](https://github.com/snakers4/silero-vad) [[1]](#1) using [ONNX Runtime Web](https://github.com/microsoft/onnxruntime/tree/main/js/web) / [ONNX Runtime Node.js](https://github.com/microsoft/onnxruntime/tree/main/js/node).

The only problem that this folk solve is documentation. `@ricky0123/vad` does not have proper docs to implement things.

Real documentation for bundling the voice activity detector for the browser or using it in node or React projects can be found on [vad.ricky0123.com](https://www.vad.ricky0123.com).

this package provide's you extended docs that help you to integrate VAD in any JS framework.

highly recommanded to visit there real docs first and read more from BHuman's documentation

### Quick Start

To use the VAD via a script tag in the browser, include the following script tags:

```html
<script src="https://cdn.jsdelivr.net/npm/onnxruntime-web/dist/ort.js"></script>
<script src="https://cdn.jsdelivr.net/npm/bhuman-vad-web@0.0.1/dist/bundle.min.js"></script>
<script>
  async function main() {
    const myvad = await vad.MicVAD.new({
      onSpeechStart: () => {
        console.log("Speech start detected")
      },
      onSpeechEnd: (audio) => {
        // do something with `audio` (Float32Array of audio samples at sample rate 16000)...
      }
    })
    myvad.start()
  }
  main()
</script>
```

## References

<a id="1">[1]</a>
Silero Team. (2021).
Silero VAD: pre-trained enterprise-grade Voice Activity Detector (VAD), Number Detector and Language Classifier.
GitHub, GitHub repository, https://github.com/snakers4/silero-vad, hello@silero.ai.
