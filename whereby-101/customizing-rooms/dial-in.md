# Dial-In

The Dial-in option allows meeting participants to join via a phonecall. This is particularly useful if they have internet issues, are traveling, or cannot use video for other reasons.

{% hint style="info" %}
Dial-in is currently in closed Beta and available to selected customers only.\
Email us at embedded@whereby.com to join our pilot program (terms and conditions apply).
{% endhint %}

### Per room configuration

You can configure the dial-in feature for individual rooms. Use the [POST /meetings](https://docs.whereby.com/reference/whereby-rest-api-reference/meetings) request to create the new room. During the creation request, add the following fields to the request’s body:

```json
{    
    "endDate": "2099-03-25T13:30:00.000Z",
    “isDialInEnabled”: true,
    "fields": ["dialIn"]
}
```

{% hint style="success" %}
Multiple fields can be specified in the same request, including things like [hostRoomUrl](../../reference/whereby-rest-api-reference/meetings.md)
{% endhint %}

**Example response:**

```json
{
	"startDate": "<now>",
	"endDate": "2099-03-25T13:30:00.000Z",
	"roomName": "/example-room”,
	"roomUrl": "https://example.whereby.com/example-room",
	"meetingId": "1234",
	"dialIn": {
		"code": "1234567890",
		"phoneNumbers": [
			{
				"countryCode": "US",
				"phoneNumber": "111-867-5309"
			}
		]
	},
	"roomPreferences": {
		"dialIn": true
	}
}

```

### Getting details of the dial-in access codes

To obtain the dial-in details and access codes for a specific meeting, use the `meetingId` and send a [GET request](../../reference/whereby-rest-api-reference/meetings.md#meetings-meetingid) with `?fields=dialIn` as a query param. The details are provided in a text response in JSON format, and include:

* List of Dial-in phone numbers from different countries that participants can call to join the meeting
* Dial-in access code

The above request will work for both a specific `meetingId`, or all active meetings. The response will be an array of meetings and the meetings will have the Dial-in details, but only if they were created with Dial-in enabled.

#### Creating callable links

The phone number and room code can be combined and presented as a link on your website or HTML email, allowing users to 'click' the link and dial into the room without having to manually enter the code.

To do so, create an [anchor](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/a) element with the `href` set to `"tel:<phone number>,<room code>"` like so:

`<a href="tel:111-867-5309,1234567890">Click here</a>`

{% hint style="info" %}
The comma separating the phone number from the room code above, causes the phone to wait for an answer before entering the code
{% endhint %}

Here's how that renders on this website: [Click here](tel:111-867-5309,1234567890)

### Using webhooks

Webhook events can be used to track dial-in participants. When a dialed-in participant joins or leaves the meeting, Whereby sends a [webhook](https://docs.whereby.com/meeting-content-and-quality/insights-suite-and-api/webhooks#transcription-data-properties) event with details:

* Participant’s phone number
* Participant’s date and time of joining
* Participant’s date and time of leaving the room

### Additional information

1. The Dial-in feature currently provides voice messages to participants in **English only**.
2. The Dial-in feature is **not compatible** with our [Breakout Groups](https://docs.whereby.com/whereby-101/customizing-rooms/breakout-groups-with-embedded) feature.

