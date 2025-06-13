---
description: >-
  In some more light weight or static projects, a bundler may not be required or
  sensible. Using a script tag is a great way to utilize the Whereby Web
  Component.
---

# Script Tags

When using HTML or a static project:

{% code overflow="wrap" %}
```html
<html>
    <head>
        <script src="https://cdn.srv.whereby.com/embed/v3-embed.js" type="module"></script>
    </head>
    <body>
        <div class="container">
            <whereby-embed room="some-room" />
        </div>
    </body>
</html>
```
{% endcode %}

You can use a `hostRoomUrl` instead of the `roomUrl`, if you want to give the user [host privileges](../../../user-roles-and-privileges.md#hosts)

## Web Component Features & Usage

* [Attributes of the Component](./#attributes-of-the-component)
* [Listening to events](./#listening-to-events)
* [Sending commands](./#sending-commands)
