# LocalMedia Types

## LocalMediaState

| Property                                          | Description                                             |
| ------------------------------------------------- | ------------------------------------------------------- |
| `currentCameraDeviceId?: string`                  | ID of the selected camera device                        |
| `currentMicrophoneDeviceId?: string`              | ID of the selected microphone device                    |
| `currentSpeakerDeviceId?: string`                 | ID of the selected speaker device                       |
| `cameraDeviceError: unknown`                      | If there's an error with the selected camera device     |
| `cameraDevices: MediaDeviceInfo[]`                | List of available camera devices                        |
| `isSettingCameraDevice: boolean`                  | True if camera is being set                             |
| `isSettingMicrophoneDevice: boolean`              | True if microphone is being set                         |
| `isStarting: boolean`                             | True if `localMedia` is starting                        |
| `localStream?: MediaStream`                       | Local stream of the client                              |
| `microphoneDeviceError: unknown`                  | If there's an error with the selected microphone device |
| `microphoneDevices: MediaDeviceInfo[]`            | List of available microphone devices                    |
| `speakerDevices: MediaDeviceInfo[]`               | List of available speaker devices                       |
| `startError: unknown`                             | If there's an error starting local media                |

