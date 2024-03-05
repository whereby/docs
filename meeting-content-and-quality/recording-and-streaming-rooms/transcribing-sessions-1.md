---
description: >-
  Turn the content of Whereby sessions into meeting notes, clinical
  documentation and educational material with the power of AI.
---

# Session summaries

{% hint style="info" %}
Session summaries are currently in closed Beta and available to selected customers only. Email us at embedded@whereby.com to join our beta testing program (terms and conditions apply).
{% endhint %}

Session summaries are derived from session transcriptions and provided as json responses available through the customer portal or via the API.&#x20;

In order to produce a summary from a Whereby session you need to follow this workflow:

1. Record the Whereby session using cloud recording with Whereby-provided storage. [Learn more...](recording-with-embedded/cloud-recording.md#whereby-provided-storage)&#x20;
2. Transcribe the recording. [Learn more...](transcribing-sessions.md)
3. Trigger the summary of the transcript using the desired summary template.

You can use Whereby session summaries manually through the customer portal, or programatically with the combination of API requests.

## Summary Templates

The summary template determines the structure of the summary as well as the type of information that will be derived from the transcript. You can choose from summary templates designed to be used for Whereby sessions being run in different contexts.&#x20;

The following summary templates are currently available:

* `General Bulleted`
* `General Narrative`
* `SOAP`
* `Extended SOAP`
* `Educational Lecture`
* `Educational Tutoring`

### General context

{% tabs %}
{% tab title="General Bulleted" %}
This is the default summary template designed for use in any context and in session with two or more participants. Works great for business or internal company meetings. The summary contains session agenda, key points discussed, follow up action items and a short summary. The content of each section is organised into bullet points, with the exception of the short summary, which comes as a single paragraph.

#### Sample output

{% code overflow="wrap" %}
```json
"summary": {
    "summary": "Agenda\n- Discussion of current goals and ongoing projects\n- Addressing audio issues during the meeting\n- Planning for follow-up meetings and action items\n\nKey Points Discussed\n- The team page has been updated with archive information from the past two quarters.\n- Goals for the current and next quarter were discussed, including ensuring the Progressive Web App (PWA) responds to changing network conditions, improving audio reliability, and reducing infrastructure vendor costs.\n- Audio issues during the meeting were addressed, suggesting possible electromagnetic interference.\n- The RTP quality score project is ongoing, aiming to make decisions in meetings based on connection quality.\n- The audio-only mode is being tested internally, with plans to make it automatic.\n- Infrastructure cost reduction efforts are in progress, with a knowledge-sharing session scheduled.\n- Challenges with the team include knowledge concentration, keeping the team on track, and unplanned work from business-critical customer issues.\n- Ongoing projects include glitch-free reconnect, RTP quality score, audio-only mode, audio improvements, and live captions.\n- Test DevLab is used for testing media performance, but its utility is being evaluated.\n- A new quality of service metric is being considered for Posthog.\n\nFollow Up Action Items\n- Schedule a meeting to focus on the RTP quality score project details.\n- Investigate the source of audio issues during the meeting.\n- Review and possibly revise the placement of the audio-only mode toggle for embedded customers.\n- Consider what metrics to track for the audio-only mode project.\n- Discuss resource allocation for the live captions project and RTP quality score project.\n- Attend the knowledge-sharing session on infrastructure cost reduction.\n\nShort Summary\nDuring the meeting, the team discussed the goals and ongoing projects, including improving the Progressive Web App's response to network conditions, enhancing audio reliability, and reducing infrastructure costs. Audio issues experienced during the call led to a discussion about potential electromagnetic interference. The team also talked about the RTP quality score project, which is in progress and aims to improve meeting experiences based on connection quality. Challenges faced by the team were highlighted, including knowledge concentration, staying on track, and unplanned work. Ongoing projects were reviewed, with a focus on glitch-free reconnect, audio-only mode, and live captions. Test DevLab's role in testing media performance was discussed, and a new quality of service metric for Posthog is being considered. Follow-up action items were set, including scheduling detailed project meetings, investigating audio issues, and attending a session on cost reduction."
},
```
{% endcode %}
{% endtab %}

{% tab title="General Narrative" %}
This is the default summary template designed for use in any context and in session with two or more participants. It provides a detailed summary of the session in the narrative form containing a few paragraphs. The structure of the paragraphs is derived from the content of the transcript.

#### Sample output

{% code overflow="wrap" %}
```json
"summary": {
    "summary": "Introduction: The meeting involved a discussion on various tasks related to the accessibility and functionality of a project interface, with a focus on keyboard navigation and the behavior of UI components. The participants aimed to refine tasks and ensure a clear understanding of the changes to be made to the codebase, with an emphasis on compliance with WCAG guidelines and intuitive user experience. The meeting concluded with plans to continue the discussion the next day and to document decisions regarding behavior changes for accessibility features. The participants also discussed the importance of distinguishing between mandatory accessibility fixes and nice-to-have features. The meeting involved Speaker 0 (Andres), Speaker 1, Speaker 2 (James), and Speaker 3 (Sarah Rose), with Speaker 0 leading the discussion on specific tasks and proposed solutions. Speaker 1 provided clarifications and suggestions, while Speaker 2 and Speaker 3 contributed insights and raised questions about the consistency and documentation of the changes. Key topics discussed included accessibility improvements, button behavior, and keyboard shortcuts. The team agreed on the behavior of the Enter and Space keys in different contexts and discussed the need for a public documentation page for accessibility features. Additionally, the team planned to start working on accessibility tickets the following Monday and aimed to refine the tickets for efficiency in development. The meeting ended with the decision to continue the discussion the following day and to potentially document behavior decisions asynchronously. Four key topics from the meeting are summarized below. Accessibility Improvements: Speaker 0 discussed adding details to tasks for accessibility improvements, such as mapping attributes to roles like 'role=heading' and using H1, H2 tags or the role attribute to maintain styling while improving accessibility. Button Behavior and Keyboard Shortcuts: The team discussed the behavior of buttons and keyboard shortcuts, proposing that the Space key should open context menus while the Enter key should perform default actions. This behavior would align with native HTML select elements. Speaker 2 (James) raised concerns about maintaining quick toggle functionality, suggesting keeping the Space key for primary actions and using Enter for context menus. Documentation and Consistency: Speaker 3 (Sarah Rose) questioned whether the decided behaviors should be standardized across the platform. Speaker 1 expressed reluctance to create a documentation page for internal use, fearing it could become outdated, but suggested a public page detailing accessibility features. Speaker 2 (James) emphasized the importance of documenting key behavior decisions. Pre-Call Device Setup: The team identified an issue where hitting Enter on the pre-call screen would inadvertently join the meeting instead of allowing the user to select a different camera or microphone. They agreed that Enter should not skip the pre-call unless the join meeting button is in focus, and planned to create a ticket to address this behavior.",
},
```
{% endcode %}
{% endtab %}
{% endtabs %}

### Medical context

{% tabs %}
{% tab title="SOAP" %}
This template is designed for use in medical sessions involving two participants (usually a doctor and a patient). It provides the summary in the SOAP format commonly used for clinical documentation, where SOAP stands for Subjective (S), Objective (O), Assessment (A) and Plan (P).

#### Sample output

{% code overflow="wrap" %}
```json
"summary": {
    "summary": "A 40-year-old patient reports long-term digestive issues, including bloating, constipation, and dietary restrictions. The doctor suspects constipation-predominant IBS or chronic constipation and recommends seeing a gastroenterologist for further evaluation, including a possible colonoscopy. A probiotic, increased water intake, and non-stimulant laxative are advised, alongside reviewing a list of foods that cause gas and bloating.",
    "SOAP": {
        "subjective": "40-year-old patient with long-standing digestive problems, experiencing daily bloating, constipation (less than once a week without laxatives), and dietary restrictions (avoiding lactose, gluten, fruits, vegetables, and beans). Previous medications to speed up stomach emptying were ineffective. Occasionally has bowel movements every 2-3 days for a few weeks to months without diet changes, but then reverts to constipation. Experiences pain with significant bloating. Allergic to pollen and dust, taking allergy medication and supplements vitamin D and B12 due to vegetarian diet.",
        "objective": "Not identified in the transcript",
        "assessment": "Doctor suspects constipation-predominant irritable bowel syndrome (IBS-C) or chronic constipation. The patient's symptoms have been persistent and are impacting daily life. The patient has a history of an upper endoscopy but no lower GI studies.",
        "plan": "The plan includes a referral to a gastroenterologist for further evaluation, possibly including a colonoscopy. The patient is advised to start a probiotic, increase water intake, and consider using a non-stimulant laxative (polyethylene glycol). The doctor will provide an informational packet on foods to avoid for gas and bloating, and suggests the patient review it for potential dietary changes. The doctor also educates the patient on lactose content in dairy products. Follow-up with the gastroenterologist and the referring doctor is recommended if the patient is not satisfied or if there are any changes in symptoms."
    }
},
```
{% endcode %}
{% endtab %}

{% tab title="Extended SOAP" %}
This template is designed for use in medical sessions involving two participants (usually a doctor and a patient). It provides the summary in the SOAP format commonly used for clinical documentation (refer to the SOAP template) expanded with additional fields: Chief Complaint, Medications and Allergies.

#### Sample output

{% code overflow="wrap" %}
```json
"summary": {
    "summary": "A female patient is experiencing headaches for the last 7 days, especially upon waking. She has a history of migraines, but her current symptoms are different. She feels congested, has dark nasal discharge, slight sore throat, and ear fullness. The doctor diagnoses a sinus infection and recommends over-the-counter treatments instead of antibiotics, as it's likely viral.",
    "extended_SOAP": {
            "chief_complaint": "Headaches for the last 7 days, especially upon waking.",
            "subjective": "Patient has a history of migraines but current headache feels different and does not respond to migraine medication. Headache intensity is 7 out of 10 upon waking, decreasing to 5 as the day progresses. Symptoms include congestion, dark nasal discharge, slight sore throat, ear fullness, and occasional lightheadedness.",
            "medications": "Migraine medication (unspecified).",
            "allergies": "Allergic to fish and seafood, causing itchy mouth and shortness of breath.",
            "objective": "Not identified in the transcript.",
            "assessment": "Likely sinus infection, probably viral in nature.",
            "plan": "Recommend over-the-counter treatments: Mucinex (mucus thinner), nasal saline spray, neti pot, and Flonase (nasal steroid). Advised to monitor symptoms and reach out if condition worsens or does not improve, at which point antibiotics may be considered."
        }
},
```
{% endcode %}
{% endtab %}
{% endtabs %}

### Educational context

{% tabs %}
{% tab title="Educational Lecture" %}
#### Sample output

<pre class="language-json" data-overflow="wrap"><code class="lang-json"><strong>"summary": {
</strong>    "summary": "Overview:\nThe session focused on transcription and its applications, such as supercharging workflows, fact-finding, source verification, content discovery, and summarization. The speaker highlighted the power of transcription and demonstrated a new feature that allows users to view and interact with transcriptions synchronized with video recordings. The session also touched upon the potential integration with other features like participant insights and the use of utterances for more nuanced transcription. Additionally, there were discussions on pre-call network tests and the possibility of incorporating enterprise-level testing tools for WebRTC infrastructure.\n\nKey Concepts:\n- Transcription is a powerful tool for content discovery and workflow enhancement.\n- Transcription APIs provide start and stop times for each word or sentence, which can be used to synchronize text with video.\n- The new feature allows viewing of recordings with synchronized transcription, enhancing the usability of cloud recordings.\n\nTranscription Features:\n- Transcription data includes speaker diarization, timestamps, and the ability to skim through videos.\n- The speaker demonstrated a new page that displays transcriptions alongside the video, allowing users to follow along karaoke-style.\n- The transcription viewer could potentially include features like search functionality, speaker IDs, and summaries.\n\nTechnical Implementation:\n- The demo used the recordings API endpoint and fetched video URLs from Cloudflare.\n- The full JSON data of the transcription is now saved, not just the markdown text.\n- The speaker discussed potential issues with loading videos and accessing Cloudflare, as well as the need to configure CORS headers correctly.\n\nPotential Enhancements:\n- Integration with participant insights to provide more context in transcriptions.\n- The use of utterances in transcription for more accurate speaker patterns and closed captioning.\n- Offering summarization as part of the transcription service.\n\nPre-call Network Test Discussion:\n- The pre-call network test is designed to detect network-related issues by setting up a peer connection and verifying data flow through the SFU.\n- There was a suggestion to explore enterprise-level testing tools for WebRTC infrastructure to better serve customers with large-scale needs.\n\nConclusion:\n- The session concluded with the idea that offering full transcription data could make cloud recordings more attractive and open up possibilities for features like searchable transcripts for educational and moderation purposes."
  },
</code></pre>
{% endtab %}

{% tab title="Educational Tutoring" %}
#### Sample output

{% code overflow="wrap" %}
```json
"summary": {
    "summary": "Lesson Overview:The lesson was a trial teaching session to demonstrate various activities for literacy and reading comprehension. It involved practicing phonics by reading and transforming letter sounds into words, identifying digraphs, and improving reading fluency through a book reading exercise. The student, Henry, demonstrated knowledge of letters and was able to create and transform words with guidance from the teacher. Additionally, the lesson included a reading comprehension discussion surrounding a character named Lisa from the book being read, exploring predictions about the story's development. Next, the teacher shared a book titled 'Super School,' reinforcing the importance of math in practical situations and discussing the moral of the story. Lastly, the student expressed interest in more challenging activities, particularly in handling digraphs and blends for the next session.\n\nPhonics Practice:\n- The student was tasked with sounding out individual letters.\n- The student successfully constructed and transformed words such as 'chat,' 'what,' 'that,' 'van,' 'thin,' 'chin,' 'shin,' 'ship,' 'sheep,' and 'steep' from given letters.\n\nWord Transformation:\n- The student shifted from creating the word 'chat' to forming new words by changing letter sounds.\n- The student demonstrated proficiency in identifying sounds and the ability to differentiate between similar sounds, such as 't' and 'th.'\n\nReading Comprehension:\n- The student read the title of the book and the chapter 'A playground for Lisa' and identified the characters and their relationship.\n- The student worked on pronunciation and self-corrected reading errors with minimal guidance, improving their understanding of the text.\n- The student predicted story events, particularly around a new character named Lisa and her ability to participate in playground activities due to her wheelchair.\n\nMath in Practical Situations:\n- The teacher introduced a book called 'Super School,' which incorporated math concepts like multiplication, division, and fractions into a superhero narrative.\n- The story highlighted how subjects taught in school, such as math, are applicable in real-life scenarios, indirectly teaching the student the importance of seemingly unrelated academic skills.\n\nPredictions and Discussion:\n- Through the reading of 'A playground for Lisa,' the student made predictions about possible outcomes in the storyline, specifically concerning the character Lisa and her interactions on the playground.\n- The student recognized and discussed potential difficulties Lisa might experience due to using a wheelchair.\n\nAction Items:\n- Continue practicing letter blends and digraphs to handle more challenging scenarios.\n- Maintain the current reading level and select materials that are suitably challenging for the student.\n- Explore more reading comprehension exercises, encouraging the student's prediction and analytical skills.\n- No additional action items were specified for addressing math education as the focus remained on literacy and reading comprehension."
},
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Manual Session Summaries

In order to use Whereby session summaries you need to first record and transcribe the session. [Learn how to transcribe Whereby sessions manually](transcribing-sessions.md#manual-transcriptions).

Go to "Transcriptions" page of your customer portal to access all existing transcriptions of Whereby sessions. Find the transcription of a session that you want to summarise and select the template which you want to use for the summary.&#x20;

The processing time, especially for longer sessions, may take up to a minute. Once the summary is generated, you can download it in the form of a .json file.

For each session transcript, only one summary can be created at a time. If you want to summarize the session using a different template, you must first delete the existing summary of the transcript. Only the most recent summary is stored and available for download.



## Programmatic Session Summaries

If you want to automate the process of summarising Whereby sessions, you can do so with a combination of API requests.&#x20;

In order to use Whereby session summaries you need to first record and transcribe the session. [Learn how to setup an automated workflow for session transcriptions.](transcribing-sessions.md#programmatic-transcriptions)

#### Select the transcript to summarise

Summaries are derived from session transcriptions, so you need to find the `transcriptionId` of the session to be summarised.

You can get the `transcriptionId` of an individual session from the `transcription.finished` [webhook](../../reference/webhooks.md#transcription-data-properties) event. Alternatively, you can fetch the list of all transcriptions with a [GET /transcriptions](../../reference/whereby-rest-api-reference.md#transcriptions-1) request.

#### Trigger the summary process

In order to summarise the transcript, send a [POST request to /summaries](../../reference/whereby-rest-api-reference.md#summaries-2) endpoint with the `transcriptionId` and the desired summary template in the request body:

```json
{ 
    "transcriptionId": "1a74b982-deec-4615-b1d1-3d38a37d6698", 
    "template": "General Narrative" 
}
```

The `"template"` field is optional, and - if not set - the "General Bulleted" template will be used by default.&#x20;

You expect a `201 Created` response with the `summaryId:<id>` content.

For each session transcript, only one summary can be created at a time. If you want to summarize the session using a different template, you must first delete the existing summary of the transcript. Only the most recent summary is stored and available for download.

#### Fetch the summary

You can fetch an individual summary with a [GET /summaries/{summaryId} ](../../reference/whereby-rest-api-reference.md#summaries-summaryid)request.&#x20;

Alternatively, you can fetch all summaries with a [GET /summaries](../../reference/whereby-rest-api-reference.md#summaries) request, witch returns a paginated response containing 25 items per page.

The summarisation process is running in the background and usually takes about a minute to complete. There is no webhook event sent when the summary is finished, so you need to poll the endpoint and read the `state` field of the response.

For `state: in_progress` the `summary` field is empty. Once the process is completed, the `state` field changes to `ready` and the `summary` field contains the desired content.

#### Delete the summary

Session summaries are stored in Whereby database until you delete them.

You can delete a summary with a [DELETE /summaries/{summaryId} ](../../reference/whereby-rest-api-reference.md#summaries-summaryid-1)request where you expect a `204 No Content` response.

## Known limitations

{% hint style="warning" %}
Session transcriptions and summaries are only available for session recordings stored in Whereby-provided storage. If you store session recordings in your own Amazon S3 bucket, it will be not possible to trigger transcriptions and summaries for these recordings. [Learn more how to set-up cloud recording to use Whereby-provided storage.](recording-with-embedded/cloud-recording.md#setup)
{% endhint %}

