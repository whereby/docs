---
description: >-
  More than ever, workshops, lectures and conferences are happening online. So
  it's never been more important to combine bigger (often one-way) meetings with
  smaller, collaborative sessions.
---

# Breakout Groups with Embedded

## Implementing Breakout Groups

With Breakout Groups in Whereby Embedded you can easily implement collaborative sessions directly into your app or website.

To use Breakout Groups you'll need to ensure the following is true. All of these conditions must be met for Breakout Groups to function properly, so if you're having issues implementing Breakout Groups these are good things to double check:

* The meeting is set to the `group` roomMode during [meeting creation](../whereby-rest-api-reference.md#create-meeting)&#x20;
* You have the top toolbar enabled. Normally this is enabled by default, but if you're using the [`?minimal`](using-url-parameters.md#minimal) parameter it will need to be manually re-enabled with [`?topToolbar=on`](using-url-parameters.md#toptoolbar-less-than-on-or-off-greater-than)
* You are using the [`?breakout=on`](using-url-parameters.md#breakout-less-than-on-or-off-greater-than) parameter
* The meeting will happen between the meeting creation and `endDate` that was defined in the API request to create the room
* You are using [Host URLs](../user-roles-and-privileges.md) and will have a Host in the meeting

## Using Breakout Groups

Once you have Breakout Groups implemented, it will be important that Hosts know how to use them. For this, we have a series of guides that all Hosts should review before getting started:

{% tabs %}
{% tab title="Start Breakout Groups" %}
1. Hover over the 'people' button to start your breakout groups\
   <img src="../.gitbook/assets/BreakOut Groups start.png" alt="" data-size="original">
2. The main room can hold up to 200 people. You can split these people into a maximum of 20 groups and can change the group names to suit any situation.\
   <img src="../.gitbook/assets/Screenshot 2023-04-14 at 2.03.06 PM.png" alt="" data-size="original">
3. Drag and drop participants to assign groups, or shuffle everyone to save time and encourage connection.
4. When you're ready, select "Start breakout session"
{% endtab %}

{% tab title="Group Assignments" %}
1. Before selecting start, drag and drop participants names to assign them to a group.
2. Or, select "Assign all" to have Whereby do the work for you. You can press "Shuffle" to re-assign the groups until you're happy with the allocation.
3.  Leave your participants in the unassigned section to allow them the freedom of choice.\


    <figure><img src="../.gitbook/assets/file-4VezVUDlFq.gif" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Communicating with Participants" %}
When leading group sessions, it can be helpful to communicate with everyone at once to remind them of topics, encourage discussion, or bring them back to the main room. You can broadcast to all groups via chat or by getting on stage. Messages sent within a breakout group will only be seen by the people in that group.

1.  Check the "Broadcast to all groups" box in the chat to send everyone a written message.

    ![](../.gitbook/assets/file-qnSyU1TEKF.png)
2.  Select "Go on stage" to communicate with everyone via audio.\


    <figure><img src="../.gitbook/assets/file-Ze9qbAh00B.png" alt=""><figcaption></figcaption></figure>
3.  While "on stage" all groups will be able to hear you. You can get "off stage" to stop broadcasting to all groups.\


    <figure><img src="../.gitbook/assets/file-JdkbZMehFm.png" alt=""><figcaption></figcaption></figure>


{% endtab %}
{% endtabs %}

\




\
