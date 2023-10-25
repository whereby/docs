---
description: >-
  Transcribing allows you to get a verbatim transcript of what was said in the
  Whereby meeting.
---

# Transcribing sessions

Transcriptions are derived from session recordings stored in Whereby-provided storage and saved as text files accessible through the customer portal or via the API. They can be used as a standalone resource (eg. for compliance purposes) or sent to an external service for post processing (eg. to derive key topics or create a session summary).

In order to produce a transcript from a Whereby session you need to follow these steps:

1. create a Whereby room with cloud recording enabled and configured to use Whereby-provided storage
2. record the session
3. trigger the transcription for the recording

You can use Whereby transcriptions manually through the customer portal, or programatically with the combination of API requests and webhook events.

## Manual Transcriptions

In order to use Whereby transcriptions you need to first configure meeting recordings to use cloud recording with Whereby-provided storage.  Go to “Configure” → “Recording” section of your customer portal and choose "Whereby-hosted cloud recording" option with the trigger and recording format of your choice. [Learn more about recording options](recording-with-embedded/cloud-recording.md).

{% hint style="info" %}
When configuring cloud recording options via the customer portal, they will be applied as default settings for all rooms and meetings. However, you can override the defaults by specifying different preferences in the POST requests used to create meetings.
{% endhint %}

<figure><img src="../.gitbook/assets/Screenshot 2023-09-28 at 13.55.34.png" alt=""><figcaption><p>Set "Whereby-hosted cloud recording" as Recording option</p></figcaption></figure>

When the sessions get recorded, you can access all recordings saved in the Whereby-provided storage from the “Recordings” page of your customer portal. Find the session that you want to transcribe and choose the "Start transcription" option from the Actions column.

<figure><img src="../.gitbook/assets/Screenshot 2023-09-28 at 14.00.31.png" alt=""><figcaption><p>Start the transcription for selected recording of a Whereby session</p></figcaption></figure>

The transcription process may take some time, especially for recordings longer than 30 minutes. Once the transcript is ready, you can download it. The transcript is downloaded as an .md file.

<figure><img src="../.gitbook/assets/Screenshot 2023-09-28 at 14.06.48.png" alt=""><figcaption><p>Download the transcript from the session recording</p></figcaption></figure>

You can also download or delete transcriptions that have been already created from the “Recordings” page of your customer portal.

## Programmatic Transcriptions

If you want to automate the process of transcribing Whereby meetings, you can do so with a combination of API queries and webhook events.&#x20;

#### Configure session recoding

First make sure that meetings which should be transcribed are configured to use cloud recording with Whereby-provided storage:&#x20;

* If you want to use cloud recording with Whereby-provided storage **globally for all rooms**, go to “Configure” → “Recording” section of your customer portal and choose "Whereby-hosted cloud recording" option with the trigger and recording format of your choice.
* If you prefer to set the **recording type for each room or meeting individually**, create the meeting with [POST /meetings](../whereby-rest-api-reference/#meetings-1) request using the following parameters for recording (with the `"startTrigger"` and `"fileFormat"` of your choice):&#x20;

```json
"recording": {
    "type": "cloud",
    "destination": {
      "provider": "whereby",
      "fileFormat": "mp4"
    },
    "startTrigger": "none"
   },       
```

#### Record and transcribe the session

Depending on the type of recording trigger specified for the meeting, it will be recorded automatically or the host will need to start the recording manually.&#x20;

When the recording is completed, Whereby sends a `recording.finished` [webhook](../webhooks.md#cloud-recording-data-properties) event. Hook onto that event to fetch the `recordingId` of the session that you want to transcribe.&#x20;

Then send a [POST /transcriptions](../whereby-rest-api-reference/#transcriptions-1) request with the  `recordingId` to start the transcription job.&#x20;

The transcription process may take some time, especially for recordings longer than 30 minutes. Once the transcript is ready, Whereby sends a `transcription.finished` [webhook](../webhooks.md#transcription-data-properties) event. Hook onto that event to fetch the `transcriptionId` of the session that you want to transcribe.&#x20;

Send a [GET /transcriptions/{transcriptionId}/access-link](../whereby-rest-api-reference/#transcriptions-transcriptionid-access-link) request to get the download link of the transcription file. Transcripts are downloaded as .md files.&#x20;

#### Delete the recording and transcription files

Both the recording and the transcript of the session will be stored in the Whereby-provided storage until you delete them. If you want to optimise the cost or minimise the time when your sessions' content is stored in the Whereby-provided storage you can delete the assets with 2 API requests:

* [DELETE /recordings/{recordingId}](../whereby-rest-api-reference/#recordings-recordingid-1)
* [DELETE /transcriptions/{transcriptionId}](../whereby-rest-api-reference/#transcriptions-transcriptionid-1)    &#x20;

