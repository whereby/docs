---
description: >-
  An exhaustive list of the features and capabilities available within our
  implementation options
---

# Whereby Embedded Feature Comparison

✓ - capability available out of the box

✕ - currently not available

\</> - achievable to some extent and/or with some development effort

### Common features (for all participants)

|                                        | Pre-built UI (Web Component) | React Hooks                                                                                                 |
| -------------------------------------- | ---------------------------- | ----------------------------------------------------------------------------------------------------------- |
| Pre-call: enable cam/mic               | ✓                            | ✓                                                                                                           |
| Pre-call: room knocking                | ✓                            | ✓                                                                                                           |
| Pre-call: device and connectivity test | ✓                            | ✕                                                                                                           |
| Own microphone on/off                  | ✓                            | ✓                                                                                                           |
| Own camera on/off                      | ✓                            | ✓                                                                                                           |
| Change camera                          | ✓                            | ✓                                                                                                           |
| Change microphone                      | ✓                            | ✓                                                                                                           |
| Change speaker                         | ✓                            | ✕                                                                                                           |
| Background effects                     | ✓                            | ✕                                                                                                           |
| Mirror self-view                       | ✓                            | ✓                                                                                                           |
| Hide self-view                         | ✓                            | ✕                                                                                                           |
| Picture-in-picture                     | ✓                            | ✕                                                                                                           |
| Dynamic grid layout                    | ✓                            | <p>&#x3C;/><br><br>coming soon with grid layout component</p>                                               |
| Change video tile position             | ✓                            | <p>&#x3C;/><br><br>can be coded on the client side</p>                                                      |
| Noise reduction on/off                 | ✓                            | ✕                                                                                                           |
| HD video on/off                        | ✓                            | ✕                                                                                                           |
| Widescreen video on/off                | ✓                            | ✕                                                                                                           |
| Low data mode on/off                   | ✓                            | ✕                                                                                                           |
| Audio-only mode                        | coming soon                  | ✕                                                                                                           |
| Reduce visual effects                  | ✓                            | ✕                                                                                                           |
| Gesture recognition                    | beta (upon request)          | ✕                                                                                                           |
| Closed captions                        | coming soon                  | ✕                                                                                                           |
| Remote volume control                  | ✓                            | ✕                                                                                                           |
| Share screen (send/receive)            | ✓                            | ✓                                                                                                           |
| Multi-screen sharing                   | ✓                            | ✓                                                                                                           |
| Chat: group messages                   | ✓                            | ✓                                                                                                           |
| Chat: private messages                 | ✕                            | ✕                                                                                                           |
| Chat: mention participant              | ✓                            | ✕                                                                                                           |
| Chat: replies                          | ✓                            | ✕                                                                                                           |
| Raise hand                             | ✓                            | ✕                                                                                                           |
| Emoji reactions                        | ✓                            | <p>&#x3C;/><br><br>rendering of chat messages (incl. emojis) can be fully customised on the client side</p> |
| Fullscreen view                        | ✓                            | <p>&#x3C;/><br><br>can be built on the client side</p>                                                      |
| Maximised view                         | ✓                            | <p>&#x3C;/><br><br>can be built on the client side</p>                                                      |
| Room timer                             | ✓                            | <p>&#x3C;/><br><br>can be built on the client side</p>                                                      |
| Integrations                           | ✓                            | ✕                                                                                                           |
| Edit display name                      | ✓                            | ✓                                                                                                           |
| Co-location groups                     | upon request                 | ✕                                                                                                           |
| Session quality analytics (live)       | coming soon                  | ✕                                                                                                           |

### Host features (for participants joining through host URL)

|                                 | Pre-built UI (Web Component)                                | React Hooks                                                                                                      |
| ------------------------------- | ----------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| Let in / reject knocking guests | ✓                                                           | ✓                                                                                                                |
| Message awaiting guests         | ✓                                                           | ✕                                                                                                                |
| Start / stop recording          | ✓                                                           | ✓                                                                                                                |
| Spotlight view                  | ✓                                                           | <p>&#x3C;/><br>currently can be coded on the client side, but also<br>coming soon with grid layout component</p> |
| Ask to speak if raised hand     | ✓                                                           | ✕                                                                                                                |
| Remote microphone off           | ✓                                                           | ✕                                                                                                                |
| Remote camera off               | ✓                                                           | ✕                                                                                                                |
| Remove participant              | ✓                                                           | ✕                                                                                                                |
| End meeting for all             | ✓                                                           | ✕                                                                                                                |
| Breakout groups                 | ✓                                                           | ✕                                                                                                                |
| Lock/unlock the room            | ✓                                                           | ✕                                                                                                                |
| Change room type (P2P↔︎SFU)     | <p>✕<br><br>room type is pre-defined upon room creation</p> | <p>✕<br><br>room type is pre-defined upon room creation</p>                                                      |
| Browser notifications           | ✓                                                           | ✕                                                                                                                |

### Room setup, session handling and post-processing

|                               | Pre-built UI (Web Component) | React Hooks                                           |
| ----------------------------- | ---------------------------- | ----------------------------------------------------- |
| Local recording: full scene   | ✓                            | ✕                                                     |
| Cloud recording: full scene   | ✓                            | <p>✓<br>records the default pre-built UI view<br></p> |
| Recording-based transcription | ✓                            | ✓                                                     |
| Session quality analytics     | ✓                            | ✓                                                     |
