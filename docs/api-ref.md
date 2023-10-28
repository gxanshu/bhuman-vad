# API Reference

## MicVAD

The `MicVAD` API is for recording user audio or use your own media stream in the browser and running callbacks on speech segments and related events.

### Example

```js
import { MicVAD } from "bhuman-vad-web"
const myvad = await MicVAD.new({
  onSpeechEnd: (audio) => {
    // do something with `audio` (Float32Array of audio samples at sample rate 16000)...
  },
})
myvad.start()
```

to find all the parameters and attribute that taken by this `new()` function are mentioned on [offical website](https://www.vad.ricky0123.com/docs/API/#options). 

here are some extended details about that function that may helpful for you.

`MicVAD` takes streams too so you can run VAD on a stream that you already working on. mightbe for `MediaRecorder` or anthing else.

here is an example of custom stream option with react.

```js
navigator.mediaDevices
      .getUserMedia({ audio: {
          channelCount: 1,
          echoCancellation: true,
          autoGainControl: true,
          noiseSuppression: true,
      } })
      .then(async(stream) => {
        const mediaRecorder = new MediaRecorder(stream)
        const localAudioChunks: Blob[] = []

        mediaRecorder.current.addEventListener('dataavailable', async event => {
          if (event.data && event.data.size > 0) {
            const chunk = event.data
            console.log('data')
          }
        })

        mediaRecorder.current.start()
        //@ts-ignore
        // import MicVAD from package or use vad if you are using scripts
        const myvad = await vad.MicVAD.new({
          //@ts-ignore
          onSpeechEnd: (audio) => {
            // do something with `audio` (Float32Array of audio samples at sample rate 16000)...
            console.log("user said something")
          },
          stream: stream // pass you stream here
        })
        myvad.start()
      })
      .catch(err => {
        alert("Microphone is not accessible.")
        console.log('Error: ' + err)
      })
``` 

here you can see how we are taking audio permission from user and then pass that stream to VAD.

one thing to notice is you enable some features of `getUserMedia` function so vad can work currectly

```js
.getUserMedia({ audio: {
          channelCount: 1,
          echoCancellation: true,
          autoGainControl: true,
          noiseSuppression: true,
} })
```

please turn on `echoCancellation`, `autoGainControl`, `noiseSuppression` so it can help VAD's algorithm to work perfectly.

