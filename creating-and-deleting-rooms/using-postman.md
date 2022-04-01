---
description: >-
  The Whereby Postman collection offers a quick and easy way of creating and
  managing rooms and lets you explore the API without needing to set up anything
  in your project first.
---

# Using Postman

{% hint style="info" %}
**Note**: To use the API, you’ll need to [create an API key](creating-an-api-key.md). A new key is generated from the “Configure” section in the Embedded dashboard. Your API key is secret and should only be used from your server.
{% endhint %}

If you don’t have a Postman account already, sign up for one here: [https://www.postman.com/](https://www.postman.com)

Next, open [Whereby’s Embedded API Postman collection](https://www.postman.com/wherebyhq/workspace/whereby-s-public-workspace/collection/15283449-36906103-00b9-43fa-88e4-f2ed52fdedd3?ctx=documentation)

Start by creating your own copy of the collection by clicking the “Fork” button and add the copy to your own Postman workspace

![](<../.gitbook/assets/postman 1.png>)

In your forked collection, paste your API token into the "Token" field, and then click "Save"

![](<../.gitbook/assets/postman 2.png>)

Now you're ready to create a room.&#x20;

1. Click one of the "Create room" requests in the left column
2. Click "Send"
3. You should receive a response containing meeting information and URL

![](<../.gitbook/assets/postman 3.png>)

Copy the room URL from the response and open it in your browser.

Congratulations! You have created a transient room!

{% hint style="info" %}
**Note**: If you want to test a room as a guest/visitor, it can be a good idea to open transient rooms in a private window to make sure you aren't accidentally logged into your subdomain! Being logged into your Whereby account will give you host privileges in rooms under the corresponding subdomain.
{% endhint %}
