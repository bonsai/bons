# BONS Product Requirements Document

**Status:** Draft  
**Repository:** `bonsai/bons`  
**Product:** BON ODORI NETWORK SERVICE

## 1. Vision

BONS is a nationwide hub where people discover, organize, attend, learn, and share bon odori events and festivals. It connects participants and organizers while helping more people become dancers, teachers, creators, and future cultural stewards.

BONS is community-driven: people create and maintain cultural activity for one another. Municipalities may participate as one stakeholder among many, but the service is not government-led.

## 2. Target users

### Participants
- Discover events by date, area, style, and accessibility.
- Learn dances before attending.
- Check in at events and collect participation records.
- Track practice and learning progress.
- Share dance videos if they choose.

### Organizers
- Register and update events.
- Maintain organizer and venue information.
- Share event information and participation instructions.
- Optionally run group dance tests or workshops.
- Review batch assessment results where enabled.

## 3. Scope

BONS covers all kinds of bon odori and related festivals nationwide:
- Traditional and locally rooted dances
- Neighborhood and community events
- Large-scale festivals
- DJ bon odori and contemporary/creative formats
- Practice sessions, workshops, school and educational events

Categories are non-exclusive. Use tags and provenance rather than forcing each event into one rigid category.

## 4. Core participant journey

1. Discover an event through list, map, or calendar.
2. Review event details, organizer, venue, access, admission, and source status.
3. Attend and check in using an event QR code or another organizer-approved method.
4. Receive a stamp or event medal.
5. Explore dance references and practice challenges.
6. Earn skill badges through transparent criteria.
7. Return to events and, over time, teach or contribute dances.

## 5. Event and organizer capabilities

### Event registration
- Anyone can propose or register an event.
- Store title, description, categories/tags, schedule, venue, organizer, admission, official URL, source/provenance, and status.
- Support draft, unverified, verified, cancelled, and completed states.
- Provide correction and reporting workflows.

### Discovery
- Search and filter by date, region, event type, and organizer.
- Map and calendar views.
- Clearly distinguish verified information from community-submitted information.

### Participation and collection
Keep collection concepts semantically distinct:
- **Stamp:** visit or location record.
- **Medal:** participation commemorative item.
- **Badge:** skill, practice, contribution, or achievement recognition.
- **Card:** collectible cultural information or artwork.

Check-in and collection should be useful without requiring NFTs or blockchain.

## 6. Dance learning and assessment

### 6.1 Multi-dance launch

The first release of the dance-learning experience must start with **multiple reference dances**, not a single canonical song. Initial selection should balance:
- Recognition among people in Japan
- Approachability for international newcomers
- Regional and stylistic diversity
- Availability of usable reference footage
- Rights and permissions for video, choreography documentation, and music

Do not imply that one dance represents all of Japan. Each dance has its own reference, variants, segmentation, and assessment profile. Candidate songs are to be selected through a separate survey and rights review; no initial song list is treated as finalized by this PRD.

### 6.2 Reference-data pipeline

The source of truth for assessment is not the raw video alone. Build structured, versioned dance-reference data from authorized reference footage.

Pipeline:
1. Register source videos, provenance, rights, performer consent, camera setup, and dance/variant metadata.
2. Extract frames and timestamps.
3. Detect and track the dancer.
4. Extract pose landmarks and confidence values with MediaPipe Pose Landmarker or an equivalent model.
5. Segment the dance into named movement phrases/forms.
6. Where multi-view synchronized footage is available, calibrate cameras and reconstruct a 3D pose sequence.
7. Where only monocular footage is available, store monocular 3D estimates as estimates, not measured ground truth.
8. Normalize body scale and coordinate conventions while preserving meaningful orientation and movement.
9. Validate the reference with a knowledgeable human reviewer.
10. Publish a versioned reference profile for real-time and offline assessment.

A single camera view cannot reliably reveal every hidden depth relationship. Record camera angle, occlusion, confidence, and reconstruction method. Never silently present inferred depth as ground truth.

### 6.3 Reference data model

A dance reference should contain:
- Dance ID, title, region/community, style, variant, description, source and rights metadata
- One or more reference videos and camera metadata
- Pose sequence: timestamp, 2D/3D landmarks, confidence, coordinate frame, reconstruction method
- Movement segments: start/end, phrase name, expected movement, beat alignment, repetition pattern
- Assessment profile: applicable metrics, tolerances, weights, and known variants
- Reviewer, validation status, version, and change history

### 6.4 Real-time participant assessment

Use browser JavaScript and MediaPipe to process the participant's camera stream locally where feasible:
- Detect pose landmarks in real time.
- Align the participant's movement to the selected reference segment.
- Compare joint angles, relative positions, orientation, timing, and movement trajectory.
- Show a skeleton overlay and simple actionable feedback.
- Handle camera mirroring and different camera placements explicitly.
- Display uncertainty or request repositioning when landmarks are occluded or confidence is low.

A 2D webcam observation must not be treated as if it fully observes 3D motion. Compare in a camera-aware representation and only score dimensions supported by the available evidence.

### 6.5 Offline VLM assessment

For uploaded practice videos, combine:
- Pose/time-series analysis for measurable movement and timing
- VLM review for high-level movement descriptions and contextual feedback
- Human review for ambiguous, disputed, or high-stakes outcomes

VLM output is advisory. It must not override missing evidence or invent precision. Provide feedback and confidence/quality indicators rather than claiming objective artistic truth.

### 6.6 Scoring principles

Potential dimensions:
- Choreography sequence and movement-form similarity
- Rhythm and timing
- Posture and joint configuration
- Continuity and movement transitions
- Expressive qualities, only where a dance-specific rubric can responsibly define them

Rubrics are dance- and variant-specific. Do not compare raw scores across different dances as if they shared one universal scale. Respect regional variation, individual expression, disability, age, body shape, clothing, and camera constraints. Provide accessible alternatives and avoid penalizing a person for movements they cannot perform.

### 6.7 Badges

Separate score calculation from badge issuance. Badges are issued against explicit, versioned criteria:
- First challenge completed
- Practice streak or repeated attempts
- Basic phrase completed
- Personal improvement
- Dance-specific proficiency
- Community contribution or teaching

An AI score alone should not confer an authoritative cultural qualification. Early badges should describe practice milestones, not certify cultural authenticity. Keep an audit trail for score, rubric version, reviewer override, and badge issuance.

### 6.8 Batch assessment

For workshops, schools, and organizers:
1. Upload multiple videos or submit a batch manifest.
2. Associate each clip with participant consent, dance, reference version, and challenge.
3. Run asynchronous assessment jobs.
4. Present a results table with status, score breakdown, and confidence.
5. Route uncertain cases to human review.
6. Let an authorized organizer confirm eligible practice badges in bulk.
7. Record the actor, timestamp, rubric version, and reason for every issuance or correction.

Do not expose participant videos or scores to other participants by default.

### 6.9 YouTube and video sharing

YouTube publication is optional and separate from assessment:
- Explicit opt-in by the uploader.
- Preview title, description, visibility, and selected video before upload.
- Upload only after the user confirms.
- Store the resulting URL only when the user chooses to link it to BONS.
- Support private/unlisted/public choices where the YouTube API and account permissions allow.
- Never require public posting to take a test or earn a practice badge.
- Obtain appropriate consent from all identifiable performers, and guardian consent where applicable for minors.
- Review music, choreography, recording-location, and performer rights before publication.

## 7. Privacy, safety, rights, and governance

- Obtain clear consent before recording, processing, retaining, or sharing identifiable video.
- Explain which processing happens in-browser and which data is uploaded to a server.
- Minimize retention; provide deletion and access controls.
- Treat minors' video and profile data with heightened protection.
- Keep private practice videos private by default.
- Record provenance and licenses for reference footage, music, and choreography materials.
- Provide correction, appeal, and human-review paths.
- Do not use AI scores to rank cultural authenticity or the worth of a community.

## 8. MVP priorities

### Core BONS MVP
- Event registration and editing
- Event listing/search
- Map and calendar
- Organizer profiles
- QR check-in
- Stamps and medals
- Source status and moderation

### Dance MVP
- Multiple dance references from launch
- Reference video registration and rights metadata
- Frame extraction and MediaPipe pose extraction
- Movement segmentation
- Initial 3D reconstruction experiment using synchronized multi-view footage
- Browser JavaScript + MediaPipe real-time pose comparison
- Dance-specific feedback and practice milestones
- Basic asynchronous batch processing

VLM-assisted review, advanced 3D reconstruction, and YouTube upload are staged after the basic end-to-end path is validated.

## 9. Success metrics

- Registered events and active organizers
- Event discovery-to-attendance conversion
- QR check-ins and repeat attendance
- First-time bon odori participants
- Number of reference dances and validated variants
- Dance challenge completion and repeat practice
- Improvement over a participant's own prior attempts
- Badges earned through transparent criteria
- Participation in real-world events after online practice
- People who go on to teach, organize, document, or create dances

North-star outcome: more people actively participate in and help sustain living bon odori culture.

## 10. Architecture principles

- BONS is an independently deployable application.
- Keep event discovery, dance reference, assessment, and collection domains modular.
- Use stable IDs and versioned schemas.
- Keep raw media separate from derived pose data.
- Support JSON/JSONL export for research and interoperability.
- Keep assessment jobs asynchronous and observable.
- Prefer local/browser processing for real-time feedback when practical.
- Treat reference datasets and rubrics as versioned cultural records, not disposable model prompts.

## 11. Open decisions

- Final selection and rights clearance of the initial multiple reference dances
- Whether the first multi-view reference capture uses synchronized phones or a fixed multi-camera setup
- 3D pose representation and validation protocol
- Browser performance targets and supported devices
- Video retention policy and storage provider
- Initial dance-specific scoring rubric and human reviewer process
- Whether batch assessments are included in the first public pilot or the next iteration
