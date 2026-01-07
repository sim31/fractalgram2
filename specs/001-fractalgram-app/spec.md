# Feature Specification: Fractalgram Web App

**Feature Branch**: `001-fractalgram-app`  
**Created**: 2026-01-07  
**Status**: Draft  
**Input**: User description: "Build a web app that implements fractalgram: https://github.com/sim31/frapps/blob/v1.2/concepts/fractalgram.md ..."

## User Scenarios & Testing *(mandatory)*

<!--
  IMPORTANT: User stories should be PRIORITIZED as user journeys ordered by importance.
  Each user story/journey must be INDEPENDENTLY TESTABLE - meaning if you implement just ONE of them,
  you should still have a viable MVP (Minimum Viable Product) that delivers value.
  
  Assign priorities (P1, P2, P3, etc.) to each story, where P1 is the most critical.
  Think of each story as a standalone slice of functionality that can be:
  - Developed independently
  - Tested independently
  - Deployed independently
  - Demonstrated to users independently
-->

### User Story 1 - Join and Vote with Hidden Results (Priority: P1)

Participants join a live room via a secret invite link. After logging in via the executive app, a participant sees the current poll for the active step but cannot see poll results until they cast a vote. Once they vote, they see live percentages, vote counts, and voter names for each option. They may change their vote at any time while the room is live.

**Why this priority**: Core mechanic of consensus building; without voting with hidden results there is no fractalgram process.

**Independent Test**: Using a live room with 3 test users, verify result-visibility gating and vote-change behavior without host controls or presets.

**Acceptance Scenarios**:

1. **Given** a participant is authenticated and in a live room, **When** they have not yet voted on the current poll, **Then** poll results are hidden.
2. **Given** a participant has just voted, **When** viewing the current poll, **Then** they see percentages, counts (`votes/total_participants`), and voter names per option.
3. **Given** a participant has voted, **When** they change their vote, **Then** aggregated results update accordingly for all who have voted.

---

### User Story 2 - Create Room and Credit Deduction (Priority: P1)

A user with sufficient respect credits creates a room. Creating a room deducts 5 credits. The creator configures prompt, session length, grace period, respect distribution, executive app target, and whether to elect a delegate, or selects a preset. A secret invite link is generated.

**Why this priority**: Enables rooms to exist; ties to credits and presets.

**Independent Test**: Creator with adequate credits can create a room, see credits deducted, and get a secret link without needing host step controls.

**Acceptance Scenarios**:

1. **Given** a creator has ≥5 respect credits, **When** they create a room, **Then** 5 credits are deducted and a secret link is generated.
2. **Given** a creator selects a preset, **When** creating a room, **Then** the configuration fields are auto-filled according to the preset.

---

### User Story 3 - Host Controls Step Progression (Priority: P2)

The current host can advance to next step or go back, synchronizing the step view for all participants, except when there is no clear winner for a poll. Host can transfer hosting to another participant.

**Why this priority**: Ensures orderly consensus flow and synchronization.

**Independent Test**: With 3 users and 1 host, verify next/back, sync, and host transfer without delegate election.

**Acceptance Scenarios**:

1. **Given** a poll has a clear winner, **When** the host advances, **Then** all participants’ UIs move to the next step.
2. **Given** a host transfers hosting, **When** the new host acts, **Then** step control reflects the new host.

---

### User Story 4 - Delegate Election and Results Submission (Priority: P2)

If configured, a delegate election poll runs after level polls. The results step shows the mapping of levels to participants and the elected delegate (if applicable). If an executive app is selected, participants can submit results.

**Why this priority**: Completes end-to-end loop from room coordination to executive app submission.

**Independent Test**: Run a full flow with 4 users, elect delegate, see results, and submit to a configured executive app stub.

**Acceptance Scenarios**:

1. **Given** delegate election is enabled, **When** the election poll completes, **Then** the results step includes the delegate.
2. **Given** an executive app is configured, **When** on results step, **Then** a submit button is available and sends the final mapping to the executive app endpoint.

---

### User Story 5 - Chat with Events Log (Priority: P3)

Participants can chat alongside each poll step. The chat shows user messages and an events log (votes cast, vote changes, host step changes). A "Hide events" checkbox collapses event entries, showing only user messages.

**Why this priority**: Enhances coordination and transparency while preserving privacy of specific choices until a user votes.

**Independent Test**: With 3 users, verify message publishing, event entries for actions, and the Hide events toggle.

**Acceptance Scenarios**:

1. **Given** a participant changes a vote, **When** the events log is visible, **Then** an entry appears that they voted (without revealing option until the viewer has voted on that poll).
2. **Given** Hide events is checked, **When** viewing chat, **Then** only user messages are shown.

### Edge Cases

- Tie or no clear winner in a poll prevents host from advancing; provide guidance to revote or re-discuss. [NEEDS CLARIFICATION: tie-break mechanism]
- Session timer expiry moves room to locked mode; UI shows negative time during grace period and then disables actions.
- Network interruptions: reconnect restores state, votes, and step position.
- Host leaves room: hosting persists and can be transferred when they return; if host disconnects, no automatic advance.
- Executive app unavailable: submission retriable with clear error, does not alter locked state.
- Participants joining after lock are observers only; no new participants counted in totals.
- Missing human-friendly name from executive app triggers name prompt on first login.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: The system MUST support login via an executive app and ingest identity and respect score. If no human-friendly name is provided, prompt the user to enter a display name.
- **FR-002**: The system MUST allow eligible users to create a room and MUST deduct 5 respect credits upon successful creation.
- **FR-003**: The system MUST generate a secret invite link for each room and restrict participation to holders of the link while the room is live.
- **FR-004**: The system MUST allow configuration of prompt, session length, grace period, respect distribution (default Fibonacci starting at 5; option to multiply by 2), executive app target (or None), and "Elect delegate" option; or selection of a predefined preset.
- **FR-005**: The system MUST run stepwise polls for levels (6→1), excluding already-awarded participants; the last remaining participant automatically receives the last level without a poll.
- **FR-006**: The system MUST hide poll results from a participant until they cast a vote on that poll; after voting, show percentages, `votes/total_participants`, and voter names per option.
- **FR-007**: The system MUST allow participants to change their vote while the room is live and before lock.
- **FR-008**: The system MUST provide host controls to move to next/previous step, synchronize views for all participants, and transfer hosting; advancing is blocked if no clear winner exists.
- **FR-009**: If enabled, the system MUST run a delegate election poll after level polls and include delegate in final results.
- **FR-010**: The system MUST present a results step summarizing level-to-participant mapping and delegate (if any), and MUST provide a button to submit results to the configured executive app.
- **FR-011**: The system MUST display a countdown timer for the session; after session length, continue into grace period with negative time in red; after grace, the room becomes locked and actions (votes, chat) are disabled.
- **FR-012**: In locked mode, the system MUST allow anyone with the link to view historical steps and final results as observers; state is immutable.
- **FR-013**: The system MUST include a chat visible across steps, supporting user messages and an events log for votes, vote changes, and host step changes; provide a "Hide events" toggle to suppress event entries.
- **FR-014**: The system MUST maintain participant privacy by not revealing a specific participant’s choice to another participant until the viewer has voted on that poll.
- **FR-015**: The system SHOULD be mobile-friendly and provide a responsive layout, with chat optionally hidden by default on mobile.

*Unclear/Policy Requirements:*

- **FR-016**: Respect credits source-of-truth and deduction protocol [NEEDS CLARIFICATION: Is deduction done locally in this app, or via executive app transfer/verification?]
- **FR-017**: Executive app authentication and data submission protocol [NEEDS CLARIFICATION: Which auth mechanism and API schema are used?]
- **FR-018**: Tie-break policy for "no clear winner" [NEEDS CLARIFICATION: e.g., revote, runoff, or host-declared resolution?]

### Key Entities *(include if feature involves data)*

- **User**: External identity from executive app; attributes include external ID, display name, respect score.
- **RespectAccount**: Tracks `respect`, `credits_used`, computed `respect_credits = respect - credits_used`.
- **Room**: Configuration (prompt, session length, grace, distribution, executive app, elect delegate, preset used), status (live, grace, locked), secret link token, host ID.
- **Poll**: Step type (level or delegate), options (participants), status, and results.
- **Vote**: Voter ID, option selected, timestamp, and current-effective status.
- **Message**: Chat text, author, timestamp.
- **Event**: Action log entries (vote cast/changed, step advanced/rewound, host transfer) with timestamps.
- **Result**: Final mapping of levels to participants and optional delegate.
- **ExecutiveApp**: Target endpoint configuration and submission state.
- **Preset**: Named configuration snapshot applied at creation.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: 95% of participants can join a room and cast a vote within 60 seconds after receiving a secret link (on desktop and mobile).
- **SC-002**: Result-visibility gating accuracy ≥ 99.5%: no user sees poll results before they have voted.
- **SC-003**: Host step changes propagate to all connected clients within 2 seconds p95.
- **SC-004**: Rooms reliably lock at session end + grace; no state changes after lock (0 violations in testing over 100 sessions).
- **SC-005**: Submission to executive app succeeds on first attempt ≥ 95%, with retriable errors and clear messaging for failures.
- **SC-006**: Mobile usability: System Usability Scale (SUS) ≥ 75 for mobile participants in a pilot test.
