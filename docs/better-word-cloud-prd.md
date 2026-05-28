# Better Word Cloud PRD

## Product Summary

Better Word Cloud is a live audience activity where attendees submit short responses from their phones. Instead of only showing a traditional word cloud based on frequency, the big screen groups responses into themes, highlights important patterns, supports voting, and gives hosts useful discussion material.

Example: During a panel, the host asks, "What is the biggest blocker to adopting AI at work?" Attendees answer from their phones. The big screen shows clustered themes like "trust," "data privacy," "training," and "cost," with representative responses.

## Product Philosophy

Most event polling tools collect answers. Better Word Cloud should help a room understand itself.

The experience should make audience sentiment visible in real time, while staying lightweight enough for conferences, workshops, classrooms, social events, and company meetings.

## Goals

- Make audience sentiment visible in real time.
- Give speakers and hosts useful discussion material.
- Make open-ended participation more structured and less chaotic.
- Create a better alternative to basic polling.
- Work for professional events and lighter social prompts.
- Keep participation under 20 seconds per attendee.

## Non-Goals

- Long-form surveys.
- Deep analytics dashboard in the first version.
- Replacing full event platforms.
- Requiring AI in the MVP.
- Requiring attendee accounts.

## Target Users

### Host

Conference speaker, workshop facilitator, team lead, educator, moderator, or event organizer.

### Participant

An audience member submitting a short thought from a mobile browser.

### Viewer

Audience and speaker watching the shared screen.

## Use Cases

- Conference Q&A
- Workshop reflection
- Team retrospective
- Icebreaker prompt
- Panel discussion
- Product feedback session
- Training comprehension check
- Community event mood check

## Core User Flow

### Host Flow

1. Host creates or selects the Better Word Cloud activity.
2. Host enters a prompt.
3. Host chooses response format:
   - One word
   - Short phrase
   - Anonymous sentence
4. Host selects display mode:
   - Classic cloud
   - Theme clusters
   - Sentiment view
   - Timeline pulse
5. Host starts the activity.
6. Big screen displays QR code and prompt.
7. Responses appear live.
8. Host can switch views during the session.
9. Host can export summary.

### Participant Flow

1. Participant scans QR code.
2. Participant sees the prompt.
3. Participant submits a word or short phrase.
4. Participant may optionally vote on responses if enabled.
5. Participant sees confirmation or live result.

### Big Screen Flow

1. Waiting state shows prompt, QR code, and join URL.
2. Active state displays incoming responses.
3. Responses are grouped into meaningful themes where possible.
4. Display updates live as new responses arrive.
5. Summary appears when the session is closed.

## Functional Requirements

### Prompt Setup

- Host can enter prompt.
- Host can set response limit:
  - 1 word
  - 3 words
  - 100 characters
  - 280 characters
- Host can set submissions per participant.
- Host can enable anonymous mode.
- Host can enable voting/upvoting.
- Host can enable moderation.

### Response Collection

- Participants can submit text from mobile.
- Responses update the big screen in real time.
- Duplicate or near-duplicate terms should merge where possible.
- Profanity filter should be available.
- Host can delete individual responses.

## Display Modes

### Classic Cloud

- Larger words indicate more frequent submissions.
- Similar terms can be normalized:
  - "AI"
  - "A.I."
  - "artificial intelligence"
- Common filler words are ignored.
- The screen remains legible at conference-room distance.

### Theme Clusters

- Responses are grouped into clusters.
- Each cluster has:
  - Theme label
  - Count
  - Top words
  - Example responses
- MVP can use keyword matching.
- Later versions can use embeddings or LLM-based clustering.

### Sentiment View

- Responses are grouped as:
  - Positive
  - Neutral
  - Negative
  - Mixed/unclear
- Big screen shows distribution.
- Useful for retrospectives, product feedback, and audience mood checks.

### Timeline Pulse

- Shows responses over time.
- Useful during live talks to see what topics spike.
- Can help moderators identify moments that deserve follow-up.

## Voting

Voting is optional but strategically important.

- Participants can upvote responses or themes.
- Host can enable voting after submission phase.
- Big screen shows top-voted themes or responses.
- System prevents repeated voting from the same participant/session.

## Moderation

- Host can approve responses before display.
- Host can remove visible responses.
- System can auto-hide profanity.
- Host can close submissions while keeping results visible.

## Export

Host can export:

- PNG of final visualization.
- CSV of raw responses.
- Summary report with:
  - Prompt
  - Total responses
  - Top themes
  - Top words
  - Top voted responses

## Screen States

### Big Screen

- Waiting: prompt, QR code, join URL.
- Active: live responses and selected visualization.
- Voting: responses are locked, voting is open.
- Paused: no new responses accepted.
- Closed: final summary.
- Error: connection lost or activity unavailable.

### Mobile

- Join/loading state.
- Prompt and response field.
- Submit confirmation.
- Voting view if enabled.
- Already submitted state.
- Session closed state.

## Data Model

### Activity

```json
{
  "id": "activity_456",
  "type": "better_word_cloud",
  "roomId": "room_123",
  "title": "AI Adoption Barriers",
  "prompt": "What is the biggest blocker to using AI at work?",
  "status": "waiting | active | voting | paused | closed",
  "responseLimit": 100,
  "displayMode": "classic | clusters | sentiment | timeline",
  "anonymous": true,
  "votingEnabled": true,
  "moderationEnabled": false,
  "createdAt": "timestamp"
}
```

### Response

```json
{
  "id": "response_123",
  "activityId": "activity_456",
  "participantId": "participant_123",
  "text": "Data privacy concerns",
  "normalizedText": "data privacy",
  "themeId": "theme_1",
  "sentiment": "negative",
  "status": "visible | hidden | pending",
  "voteCount": 8,
  "createdAt": "timestamp"
}
```

### Theme

```json
{
  "id": "theme_1",
  "activityId": "activity_456",
  "label": "Privacy & Trust",
  "keywords": ["privacy", "security", "trust", "data"],
  "responseCount": 32,
  "voteCount": 41
}
```

## Success Metrics

- Response rate among joined participants.
- Average responses per session.
- Host view-switch usage.
- Voting participation rate.
- Export usage.
- Number of sessions reused by same host.
- Percentage of inappropriate responses hidden.
- Time from QR scan to submission.

## MVP Scope

### Must Have

- Host creates prompt.
- Participants submit short responses.
- Big screen updates live.
- Classic cloud display.
- Basic duplicate merging.
- Host can remove responses.
- CSV export.

### Should Have

- Theme clusters using simple keyword grouping.
- Voting on responses.
- Profanity filter.
- PNG export.

### Can Defer

- AI-powered clustering.
- Sentiment analysis.
- Advanced analytics.
- Speaker notes.
- Multi-question survey flow.

## Product Risks

- Free-text inputs can become inappropriate.
- A basic word cloud may feel generic unless the themed views are strong.
- Clustering quality may disappoint if presented as "smart" too early.
- In large groups, the screen can become visually noisy.
- Typography and animation must be handled carefully for legibility.

## Technical Notes

- Reuse the existing room/session model.
- Server owns activity state.
- Big screen subscribes to activity updates via Socket.io.
- Mobile clients emit responses.
- Store raw and normalized responses separately.
- Use tokenization, stop-word removal, frequency counts, and simple normalization for MVP.
- Add keyword-based theme clustering before adding AI clustering.

## MVP Recommendation

Do not position the first version as "AI-powered insights" unless the clustering is genuinely good. Position it as "live audience themes." Start with classic word cloud plus basic theme grouping and voting. Add AI summaries later once usage justifies the extra complexity.
