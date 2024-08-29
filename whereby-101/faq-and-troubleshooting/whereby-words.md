---
description: Terms that are used within Whereby, and what they mean
---

# Whereby Words

If you've come this far, you may be looking for more information on more specific topics related to working with Whereby. Let's start by reviewing some terminology you're likely to see throughout our docs, in our interfaces, and in our blog content.



## User Roles

### Participant

Participant is a general term that refers to _anyone_ that joins a Room. This feeds in to how Whereby does billing via Participant Minutes.

### Host

A special user role that has full control over a room, and must join with a roomKey attribute. They will be able to let participants in if the room is Locked, kick participants out, turn off a participants mic/cam, and much more. A complete list of host actions can be found [in our docs](https://docs.whereby.com/whereby-101/user-roles-and-privileges#hosts).

### Guest

Any user that joins a room via a bare roomUrl. These users can enable their mic and cam, and will have access to any features that you enable via attributes. There is not a way to "promote" a Guest to a Host other than having them re-join with a roomKey.

### Viewer

A special user role that does not have the ability to enable thier mic/cam. Users join via a viewerUrl that includes a roomKey attribute, and these users do not need host approval to enter a Locked room. A room can have up to 400 Viewers, and these do not count towards the 200 participant cap. A complete overview of Viewers can be found [in our docs](https://docs.whereby.com/whereby-101/user-roles-and-privileges#viewers).

## General Terms

### Room

Rooms are the generic term that Whereby uses for the "location" of a vide meeting. These are the URLs that Whereby's API generates, and can be accessed directly in a browser or embedded into your website or app

### Session

A Session is a specific meeting that took place in a Room. You can have multiple Sessions in any given room if you'd like, but many customers opt to generate a bespoke Room for every Session.

## In-room Controls

### Pop-out

Pop-out is a feature that lets you move your local video feed into the bottom right corner of the room. This makes more space for other video tiles to take up more screen area in the room.&#x20;

<figure><img src="../../.gitbook/assets/Screenshot 2024-08-21 at 5.18.16 PM.png" alt=""><figcaption></figcaption></figure>

### Spotlight

Spotlight allows a Host to expand a specific video tile for everyone in the room. This is great if someone is giving a presentation and should be the main focus of the video feeds.&#x20;

### People Tab

The People Tab gives an overview of the participants in the room. Hosts can also use this tab to perform the following actions&#x20;

* Disable participant devices or request them to enable mic/cam
* Start Breakout Groups and assign participants to groups
* Enable co-location and organize participants

### Breakout Groups

Breakout Groups create temporary, isolated groups where participants can have self-contained, collaborative conversations. Users in a group will only be able to hear other users in that same group, and then they can return to the main room once the breakout group session is complete.



