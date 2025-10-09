# Assistant Types

## AudioSource: <mark style="color:green;">\<Object></mark>

| Property                                                                                                        | Description                                             |
| --------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------- |
| `onData: (data:` [`RTCAudioData`](assistant-types.md#rtcaudiodata-less-than-object-greater-than)`) => void`     | Raw PCM-encoded audio samples to inject in to the room. |

## AudioSink: <mark style="color:green;">\<Object></mark>

| Property                                                                                                                                 | Description                                                                                                   |
| ---------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| `subscribe: (callback: (data:` [`RTCAudioData`](assistant-types.md#rtcaudiodata-less-than-object-greater-than)`) => void) => () => void` | Subscribe to live raw PCM-encoded audio samples received from the room delivered at 10 millisecond intervals. |

## VideoSource: <mark style="color:green;">\<Object></mark>

| Property                                                                                                         | Description                                              |
| ---------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------- |
| `onFrame: (data:` [`RTCVideoData`](assistant-types.md#rtcvideodata-less-than-object-greater-than)`) => void`     | Raw I420-encoded video samples to inject in to the room. |

## VideoSink: <mark style="color:green;">\<Object></mark>

| Property                                                                                                                                 | Description                                                                                                                                                             |
| ---------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `subscribe: (callback: (data:` [`RTCVideoData`](assistant-types.md#rtcvideodata-less-than-object-greater-than)`) => void) => () => void` | Subscribe to live raw I420-encoded video frames received from the room delivered at the input video's frame rate (e.g. 24 frames per second = every \~42 milliseconds). |

## RTCAudioData: <mark style="color:green;">\<Object></mark>

| Property                 | Description                                  |
| ------------------------ | -------------------------------------------- |
| `sampleRate: number`     | Sample rate of data (in Hertz)               |
| `samples: Int16Array`    | Raw PCM audio sample data                    |
| `channelCount: number`   | Number of PCM channels in data. Default: `1` |
| `bitsPerSample: number`  | Number of bits per sample. Default: `16`     |
| `numberOfFrames: number` | Number of frames represented in `samples`    |

## RTCVideoData: <mark style="color:green;">\<Object></mark>

| Property                    | Description                            |
| --------------------------- | -------------------------------------- |
| `width: number`             | Video width (in pixels)                |
| `height: number`            | Video height (in pixels)               |
| `data: Uint8ClampedArray`   | Raw I420 video sample data             |
| `rotation: number`          | Orientation of the video. Default: `0` |
