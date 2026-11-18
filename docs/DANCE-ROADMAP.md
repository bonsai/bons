# BONS Dance Assessment Roadmap

This roadmap turns the dance-learning requirements in [PRD.md](PRD.md) into implementation-sized work.

## Delivery sequence

### Phase A — Reference corpus and data contracts
- Select multiple initial dances through a documented survey.
- Confirm rights, permissions, provenance, and representative variants.
- Define versioned dance, video, pose-sequence, movement-segment, and rubric schemas.
- Build frame extraction and MediaPipe pose extraction.

### Phase B — 3D reference feasibility
- Capture synchronized multi-view reference footage for at least one dance phrase.
- Calibrate cameras and evaluate multi-view triangulation/reconstruction.
- Compare reconstruction with monocular MediaPipe world-landmark estimates.
- Store confidence, occlusion, calibration, and reconstruction metadata.
- Human-review reference segments before using them for scoring.

### Phase C — Real-time practice
- Build a browser JavaScript + MediaPipe pose viewer.
- Implement camera-aware 2D/3D reference projection and temporal alignment.
- Add movement phrase feedback and practice history.
- Validate on multiple dances and different camera placements.

### Phase D — Assessment, badges, and batch
- Add dance-specific scoring profiles.
- Add VLM-assisted qualitative feedback with uncertainty handling.
- Add asynchronous batch processing and review queue.
- Add criteria-based badge issuance and audit history.

### Phase E — Sharing
- Add optional YouTube integration after consent, rights, and OAuth flows are designed.
- Never make public sharing a prerequisite for participation or badges.

## GitHub issue plan

1. **[P0] Define multi-dance reference corpus and selection criteria**  
   Survey candidate dances for familiarity, accessibility, regional diversity, reference-footage quality, and rights clearance. Deliver a documented initial set of multiple dances; do not nominate a single universal standard.

2. **[P0] Define dance-reference data schema and versioning**  
   Specify Dance, Variant, ReferenceVideo, CameraSetup, PoseSequence, MovementSegment, AssessmentProfile, provenance, rights, confidence, and review status. Include JSON examples and validation rules.

3. **[P0] Build reference-video ingestion and frame extraction**  
   Register authorized videos, metadata, timestamps, frame sampling, and reproducible extraction manifests.

4. **[P0] Implement MediaPipe pose extraction pipeline**  
   Extract 2D landmarks and available world landmarks, confidence/visibility, timestamps, and tracking identity. Preserve source-frame linkage and extraction version.

5. **[P0] Define movement segmentation and dance-form annotation**  
   Create phrase boundaries, movement labels, repetitions, beat alignment, and human annotation workflow. Support variants rather than one rigid choreography.

6. **[P0] Prototype synchronized multi-view 3D pose reconstruction**  
   Capture calibrated synchronized views, associate landmarks, triangulate/reconstruct 3D motion, and quantify uncertainty. Compare with monocular estimates; document limitations and a reproducible validation set.

7. **[P1] Build versioned 3D pose reference store**  
   Store coordinate conventions, timestamps, body normalization, camera metadata, reconstruction method, confidence, and reviewer approval.

8. **[P0] Build browser real-time pose viewer with JavaScript and MediaPipe**  
   Access camera with consent, display landmarks/skeleton, handle mirrored preview and camera placement, and measure frame latency on target devices.

9. **[P1] Implement camera-aware temporal reference comparison**  
   Align participant motion to selected movement segments; compare supported joint angles, trajectories, orientation, and timing. Avoid scoring unobservable depth.

10. **[P1] Add dance-specific scoring profiles and feedback**  
    Define versioned per-dance rubrics, weighted metrics, uncertainty thresholds, and understandable improvement feedback. Validate with dance practitioners.

11. **[P1] Add VLM-assisted offline video review**  
    Combine pose/time-series evidence with VLM descriptions. Return evidence-linked feedback and confidence; route ambiguous cases to human review.

12. **[P1] Implement batch assessment jobs and review dashboard**  
    Support batch manifests, asynchronous job status, per-clip results, privacy controls, retries, and human-review queue.

13. **[P1] Implement criteria-based badges and audit trail**  
    Separate scoring from issuance. Add versioned eligibility criteria, authorized bulk confirmation, corrections, and issuance history.

14. **[P2] Design optional YouTube publishing integration**  
    Define OAuth, explicit preview/confirmation, visibility choices, rights/consent checks, and BONS URL linking. No automatic public upload.

15. **[P1] Validate assessment across dances, camera views, and participants**  
    Create a consented test protocol, compare human annotations and system outputs, evaluate reliability/fairness, and document known limitations.

## Acceptance principles

- Multiple dances are supported by the data model from day one.
- Every reference asset has provenance and rights status.
- Every 3D sequence identifies how it was reconstructed and how reliable it is.
- Monocular estimates are never mislabeled as multi-view ground truth.
- Real-time assessment reports only what the camera can reasonably observe.
- Scoring is dance-specific; scores across different dances are not treated as directly comparable.
- Badges use explicit criteria and retain an audit trail.
- Public video sharing is opt-in and never required.
