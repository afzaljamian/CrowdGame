# Mosaic Wall PRD

## Product Summary

Mosaic Wall is a live audience activity where attendees submit small pieces of content from their phones, such as selfies, uploaded photos, emojis, colors, or short text. The big screen arranges those submissions into a larger collective image, event logo, word, or visual composition.

The experience should feel instant, social, and visually satisfying: attendees scan a QR code, contribute in seconds, and watch the shared screen evolve into a final artifact.

## Product Philosophy

The value is not just that phones control a screen. The value is that a room of people can create something together without installing an app, creating an account, or learning complex controls.

Mosaic Wall should work as both a fun event moment and a memory artifact after the event.

## Goals

- Create a visually satisfying collective artifact.
- Encourage audience participation in under 30 seconds.
- Make the big screen interesting to watch as submissions arrive.
- Produce a downloadable final image after the activity.
- Support small rooms and large conference halls.
- Work without requiring attendee accounts.

## Non-Goals

- Full photo editing.
- Social network features.
- Complex user profiles.
- Perfect image reconstruction in the first version.
- Requiring attendees to install an app.

## Target Users

### Host

Event host, conference organizer, workshop facilitator, company offsite lead, teacher, or social event organizer.

### Participant

An attendee with a phone who wants to quickly contribute without setup.

### Viewer

Anyone watching the big screen, whether or not they participate.

## Use Cases

- Conference opening screen
- Company all-hands
- Offsite icebreaker
- Product launch wall
- Wedding or social event memory wall
- Hackathon team intro
- Brand booth engagement screen

## Core User Flow

### Host Flow

1. Host creates or selects the Mosaic Wall activity.
2. Host chooses mosaic type:
   - Image/logo mosaic
   - Word mosaic
   - Freeform photo wall
   - Mood/color wall
3. Host uploads or selects a target image/logo if using image mosaic.
4. Host configures allowed submission types:
   - Photo upload
   - Camera selfie
   - Emoji
   - Short text
   - Color tile
5. Host starts the session.
6. Big screen displays QR code and join link.
7. Submissions appear live.
8. Host can pause, moderate, or close submissions.
9. Host exports the final wall as an image.

### Participant Flow

1. Participant scans QR code.
2. Participant sees a simple prompt.
3. Participant submits one item:
   - Take or upload photo
   - Choose emoji
   - Enter short text
   - Pick color
4. Participant sees confirmation.
5. Participant may submit again if the host allows it.

### Big Screen Flow

1. Waiting state shows QR code, join URL, and activity title.
2. Active state shows the mosaic filling live.
3. Progress is visible.
4. Final state reveals the completed mosaic.
5. End state can show a download QR code or event branding.

## Functional Requirements

### Activity Creation

- Host can create a Mosaic Wall session.
- Host can select a target image for mosaic generation.
- System supports PNG and JPG uploads.
- SVG support can be added by converting SVG to raster.
- Host can set session title.
- Host can configure prompt text.
- Host can configure allowed submission types.
- Host can set max submissions per participant:
  - 1
  - 3
  - Unlimited
- Host can enable or disable moderation.

### Participant Submission

- Participant can upload an image from phone.
- Participant can use camera capture where browser support exists.
- Participant can choose from an emoji or sticker set.
- Participant can submit short text up to 30 characters.
- Participant can pick a color tile.
- Submission should complete in under 5 seconds on a normal mobile connection.

### Mosaic Rendering

- Big screen displays submitted tiles in real time.
- For image/logo mosaic, system maps submissions to target image regions.
- Initial implementation can use a simplified approach:
  - Convert target image into a grid.
  - Calculate average color per grid cell.
  - Match submitted tiles by dominant color where possible.
  - Use fallback placement when no good match exists.
- For freeform photo wall, submissions appear in a clean grid or flowing collage.
- For word mosaic, tiles arrange to form large text.

### Moderation

- Host can remove inappropriate submissions.
- Host can pause incoming submissions.
- Host can clear all submissions.
- Host can hide participant names by default.
- Optional future version can add profanity and image moderation.

### Export

- Host can export final mosaic as PNG.
- Export should include:
  - Final mosaic
  - Activity title
  - Event name/date if configured
- Optional future version can export raw submissions as ZIP.

## Screen States

### Big Screen

- Waiting: QR code, join URL, activity title.
- Active: mosaic fills live.
- Paused: no new submissions appear.
- Complete: final mosaic reveal.
- Error: connection lost or upload issue.

### Mobile

- Join/loading state.
- Submission form.
- Upload progress.
- Confirmation.
- Already submitted state.
- Session closed state.

## Data Model

### Activity

```json
{
  "id": "activity_123",
  "type": "mosaic_wall",
  "roomId": "room_123",
  "title": "Build the Summit Wall",
  "prompt": "Add your photo to the wall",
  "status": "waiting | active | paused | closed",
  "targetImageUrl": "/uploads/target.png",
  "submissionTypes": ["photo", "emoji", "text", "color"],
  "maxSubmissionsPerParticipant": 1,
  "moderationEnabled": true,
  "createdAt": "timestamp"
}
```

### Submission

```json
{
  "id": "submission_123",
  "activityId": "activity_123",
  "participantId": "participant_123",
  "type": "photo | emoji | text | color",
  "contentUrl": "/uploads/photo.jpg",
  "text": null,
  "color": "#FFAA00",
  "dominantColor": "#CC8844",
  "status": "pending | approved | rejected",
  "createdAt": "timestamp"
}
```

## Success Metrics

- Participation rate among joined users.
- Submission completion time.
- Total number of submissions.
- Percentage of attendees watching until reveal.
- Export/download usage.
- Host reuse rate.
- Session failure rate.

## MVP Scope

### Must Have

- Host starts Mosaic Wall activity.
- Participants join by QR code or link.
- Participants upload photo or choose emoji/color.
- Big screen updates live.
- Host can close session.
- Final mosaic/export image is generated.

### Can Defer

- Advanced image matching.
- AI moderation.
- Participant names.
- Multi-round mosaics.
- Social sharing templates.

## Product Risks

- Photo moderation risk.
- Upload latency in large rooms.
- Mosaic may look messy if the algorithm is weak.
- Camera permissions can fail on mobile browsers.
- Public events may need stronger content controls.

## Technical Notes

- Reuse the existing room/session model.
- Server owns activity state.
- Big screen subscribes to activity updates via Socket.io.
- Mobile clients emit submissions.
- Existing upload flow can support the first version.
- Use `sharp` for image processing and dominant-color extraction.
- Local storage fallback is useful before AWS hosting.

## MVP Recommendation

Build Mood/Color Mosaic and Freeform Photo Wall first. They are easier than perfect logo reconstruction and still create strong visual payoff. Add target-image mosaic after the upload, rendering, and moderation flow is stable.
