# Quickly deploy Whereby to your domain

### Summary

We've put together a simple tool to make deploying Whereby's Pre-built experience on your site even easier. It's recommended that you use a dedicated domain or subdomain. You'll then be able to use the unique roomName's that are returned from our API.

Example API response:

{% code overflow="wrap" %}
```json
{
    "startDate": "2024-11-15T16:11:33.568Z",
    "endDate": "2024-11-29T16:11:33.044Z",
    "roomName": "/your-url-c4jx6g",
    "roomUrl": "https://your-account.whereby.com/your-url-c4jx6g",
    "meetingId": "93259794"
}
```
{% endcode %}

You can then use the `roomName` from above as a path on the domain you've deployed our webapp to:

```html
https://video.yoursite.com/your-url-c4jx6g
```

### Deploy the tool

1. Go to our webapp repo and fork for your own customizations and website
   1. [https://github.com/whereby/embed-wrapper](https://github.com/whereby/embed-wrapper)
2. Deploy the app to your favourite hosting provider (e.g. [Netlify](https://www.netlify.com), [Vercel](https://vercel.com) etc)
3. In `assets/index.js` the `roomUrl` variable to include your Whereby subdomain

```javascript
const roomUrl = new URL("https://your-account.whereby.com")
```

4. In the same file, you can choose to manage the room features [available as attributes](../whereby-101/customizing-rooms/using-url-parameters.md) by using the [setAttribute](https://developer.mozilla.org/en-US/docs/Web/API/Element/setAttribute) method

```javascript
whereby.setAttribute("minimal", true)
whereby.setAttribute("screenshare", false)
whereby.setAttribute("background", false)
```

