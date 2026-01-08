# Feature Specification: Fractalgram Web App
 
 **Feature Branch**: `002-fractalgram-web-app`  
 **Created**: 2026-01-07  
 **Status**: Draft  
 **Input**: User description: "Build a web app that implements fractalgram (rooms for Respect Game consensus-building, with credits, polls per level 6→1, chat, host-controlled steps, room lifetime + locked mode, optional delegate election), using a single identity app for authentication: https://eden.frapps.xyz/."
 
 ## User Scenarios & Testing *(mandatory)*
 
 ### User Story 1 - Join a room and participate in consensus polls (Priority: P1)
 
 A participant opens a random invite link to a live room, authenticates via the identity app, and participates in each step’s poll and chat to reach consensus. They can only see poll results after voting, and they can change their vote.
 
 **Why this priority**: Without joining + voting + seeing results, the app does not deliver its core value (facilitating consensus).
 
 **Independent Test**: Can be fully tested by creating a room fixture with 6 participants, having one participant join, vote, observe “results hidden until voting”, then change vote and see updated results.
 
 **Acceptance Scenarios**:
 
 1. **Given** a room invite link, **When** a user opens it and authenticates via the identity app, **Then** they are shown the current step and can participate.
 2. **Given** a poll step and a user who has not yet voted, **When** they view the poll, **Then** they do not see any vote totals, percentages, or step status details.
 3. **Given** a poll step and a user who has voted, **When** they view the poll, **Then** they see per-option percentages and `votes / total_participants`.
 4. **Given** the user has voted, **When** they change their vote, **Then** the latest vote replaces the prior one and the displayed poll results update accordingly.
 
 ---
 
 ### User Story 2 - Create a room and invite others (Priority: P2)
 
 A user with sufficient respect credits creates a room, either from a preset or by manually entering configuration fields, and receives a random invite link to share. The room starts in the first step.
 
 **Why this priority**: Rooms are the container for the consensus process; without creation + invite, the system cannot be used in real sessions.
 
 **Independent Test**: Can be tested by using a logged-in user with sufficient credits, creating a room from a preset, and verifying the invite link opens the created room.
 
 **Acceptance Scenarios**:
 
 1. **Given** a user with `respect_credits >= room_creation_cost`, **When** they create a room, **Then** `room_creation_cost` credits are deducted (recorded as credits used) and a random invite link is displayed.
 2. **Given** a user with `respect_credits < room_creation_cost`, **When** they attempt to create a room, **Then** creation is blocked and the UI explains the required credits and current balance.
 3. **Given** the room creation form, **When** the user selects a preset, **Then** the configuration fields are populated from that preset and can be reviewed before launching.
 4. **Given** the room creation form, **When** it is displayed to a logged-in user, **Then** it shows both the user’s remaining credits and the room creation cost.
 
 ---
 
 ### User Story 3 - Manage account profile and display name (Priority: P3)
 
 A logged-in user opens their profile page to review account details (respect and credits) and edit their display name.
 
 **Why this priority**: The app must provide a user-controlled, human-friendly name for chat, polls, and member lists.
 
 **Independent Test**: Can be tested by logging in, opening the profile page, editing the name, and verifying the updated name appears in room UI.
 
 **Acceptance Scenarios**:
 
 1. **Given** a logged-in user, **When** they open their profile page, **Then** they see their respect, credits used, credits remaining, display name, and EVM address.
 2. **Given** a logged-in user, **When** they edit and save their display name, **Then** the updated name is used in the app’s room UI.
 
 ---
 
 ### User Story 4 - Host guides the process and delivers results (Priority: P2)
 
 The host advances (or goes back) between steps, ensuring there is a clear winner before advancing. At the results step, the room shows the final mapping from levels to participants, optionally includes an elected delegate, and can deliver results to an optional results destination app.
 
 **Why this priority**: Host coordination and final results submission completes the “coordinate here, vote/execute there” loop.
 
 **Independent Test**: Can be tested by assigning one participant as host, progressing through steps with unique winners, optionally editing final results, arriving at results, and performing a results delivery attempt (or manual export).
 
 **Acceptance Scenarios**:
 
 1. **Given** a poll step has a unique top-voted option, **When** the host advances to the next step, **Then** all participants immediately see the next step.
 2. **Given** a poll step has a tie for top votes, **When** the host attempts to advance, **Then** the system blocks advancement and explains that there is no clear winner.
 3. **Given** the room is at the results step, **When** members are asked “Do you agree with these results?” and a member clicks “Yes”, **Then** that member sees the submit/deliver/export action (if applicable).
 4. **Given** the room is at the results step and a results destination app is selected, **When** an eligible member triggers delivery, **Then** the system records that delivery was attempted and shows success/failure status (or provides a manual export if automated delivery is unavailable).
 
 ---
 
 ### Edge Cases
 
 - A user opens the invite link after the room is locked: they become an observer and cannot vote or send chat messages.
 - A participant tries to vote after the room’s grace period elapsed: the vote is rejected and the UI explains the room is locked.
 - Two users attempt to claim the same “participant slot” concurrently (e.g., the 6th participant joins at the same time): the system prevents exceeding the participant limit.
 - The host leaves mid-session: participants can still observe and chat until locked.
 - Network interruption during voting: user can retry; only the latest vote counts.
 - A participant changes display name mid-session: name changes apply to future displays but do not alter recorded identity.
 - A results destination app is unavailable during delivery: the app provides a clear error and allows retry without corrupting room state.
 
 ## Requirements *(mandatory)*
 
 ### Functional Requirements
 
 #### Identity, Login, and Respect Credits
 
 - **FR-001**: System MUST authenticate all users via a single identity app: https://eden.frapps.xyz/.
 - **FR-002**: System MUST retrieve a user’s unique identifier (EVM address) and respect score from the identity app after login.
 - **FR-003**: If the identity app does not provide a human-friendly display name, the system MUST prompt the user to enter a display name before participating.
 - **FR-004**: System MUST represent “Respect” as a non-transferable score/opinion attached to an account identity.
 - **FR-005**: System MUST compute `respect_credits = respect - credits_used` for an authenticated user.
 - **FR-006**: System MUST uniquely identify users by their EVM address.
 - **FR-007**: System MUST block room creation if `respect_credits < room_creation_cost`.
 
 #### Room Creation and Configuration
 
 - **FR-008**: System MUST allow a user to create a room and set them as the initial host.
 - **FR-009**: System MUST define `room_creation_cost` as a build-time configurable value.
 - **FR-010**: Creating a room MUST cost exactly `room_creation_cost` credits (recorded as “credits used” for the creator).
 - **FR-011**: When showing room creation UI to a logged-in user, the system MUST show the user’s remaining credits and the `room_creation_cost`.
 - **FR-012**: Room creation MUST allow selecting a preset configuration (predefined at build time) and launching a room based on that preset.
 - **FR-013**: Room creation MUST allow manual configuration of:
   - Prompt for contribution
   - Session length
   - Grace period
   - Respect distribution (predefined options + custom)
   - Results destination app (including “None”)
   - “Elect delegate” option
 - **FR-014**: Each room configuration field MUST include a short helper description suitable for non-expert users.
 - **FR-015**: After room creation, the system MUST generate a random invite link that is hard to guess and is sufficient to access the room.
 
 #### Room Membership and Roles
 
 - **FR-016**: A room member MUST mean a user has joined the room, regardless of whether they are a participant or observer.
 - **FR-017**: System MUST distinguish between “participants” (can vote/chat while live) and “observers” (read-only).
 - **FR-018**: System MUST support exactly one host role per room at any time.
 - **FR-019**: Host MUST be able to transfer host status to another current participant, but only while the room is live.
 - **FR-020**: While a room is live, users opening the invite link MUST become participants until the participant limit is reached; after that, they MUST become observers.
 - **FR-021**: The system MUST enforce a participant count between 3 and 6 inclusive for each room.
 - **FR-022**: Host MUST NOT be able to proceed forward from a poll step if the room currently has fewer than 3 participants.
 
 #### Steps, Polls, and Eligibility (Fractalgram)
 
 - **FR-023**: System MUST represent a room as a sequence of steps that all participants view in sync.
 - **FR-024**: System MUST create poll steps for electing participants for levels 6 through 2, in order; There can be less steps if there are less than 6 participants.
 - **FR-025**: System MUST NOT create a poll step for the final level; the remaining eligible participant MUST be assigned the last level automatically.
 - **FR-026**: In each level poll, the options MUST be created from the room’s participant list (including connected and disconnected participants) excluding participants already assigned a higher level.
 - **FR-027**: If “Elect delegate” is enabled, system MUST include an additional poll step asking “Who should be elected as a delegate of this group?” with all participants as options.
 - **FR-028**: The final step MUST always be “Results”.
 - **FR-029**: Results MUST be derived from the winners of prior polls (and implicit final level assignment), producing a mapping from levels to participants; if delegate election is enabled, results MUST include the delegate.
 
 #### Voting and Visibility Rules
 
 - **FR-030**: Participants MUST be able to cast a vote in the current poll.
 - **FR-031**: Participants MUST be able to change their vote; only the latest vote per participant per poll counts.
 - **FR-032**: Poll results MUST NOT be shown to a participant until they have voted on that poll.
 - **FR-033**: After voting, poll results MUST show for each option:
   - Visual representation of vote percentage
   - Numeric percentage
   - `votes / total_participants`
 - **FR-034**: Member identities shown in the poll UI MUST use a friendly display format; the participant’s EVM address MUST also be shown in shortened form.
 - **FR-035**: The system MUST define “clear winner” for host advancement as: one option has a strictly higher vote count than every other option (no tie for first place).
 
 #### Host Step Control
 
 - **FR-036**: Host MUST be able to move the room forward to the next step (except after the last step).
 - **FR-037**: Host MUST be able to move the room back to the previous step (except before the first step).
 - **FR-038**: Host MUST NOT be able to advance past a poll step unless there is a clear winner.
 - **FR-039**: When host changes the step, all connected participants and observers MUST see the new step state.
 - **FR-040**: Before proceeding to the results step, host MUST be able to open an editable results form (ranking and delegate if present), and either proceed with edits or cancel.
 
 #### Chat and Event Log
 
 - **FR-041**: System MUST provide a chat panel available in every step (same conversation across steps).
 - **FR-042**: Users MUST be able to send messages while the room is live.
 - **FR-043**: The chat view MUST include an event log of consensus-building actions (vote cast, vote changed, host step change).
 - **FR-044**: Vote events MUST indicate that a participant voted (or changed vote) but MUST NOT reveal which option they voted for.
 - **FR-045**: Chat MUST offer a per-user “Hide events” toggle which hides event log entries and shows only user messages.
 - **FR-046**: Chat MUST be hideable/collapsible, and on mobile it SHOULD default to hidden.

 #### Member List, Step Status, and Outline Views

 - **FR-047**: The UI MUST provide a member list view available at all times while viewing a room.
 - **FR-048**: The member list view MUST be hideable, and on mobile it SHOULD default to hidden.
 - **FR-049**: The member list view MUST be divided into a participant list and an observer list.
 - **FR-050**: The participant list MUST show each participant’s display name in a friendly format, and MUST show their EVM address in shortened form.
 - **FR-051**: The participant list MUST show per-participant status:
   - Connected / disconnected (whether they currently have the room open)
   - Voted / not voted (whether they have voted in the current step)
 - **FR-052**: The host MUST be able to manage participants from the participant list:
   - Kick out participants
   - Make them observers
   - Transfer hosting rights
   - Add new participants even if they have not joined, either by entering participant details or by selecting from observers
 - **FR-053**: Observers MUST be shown in the observer list only when they are currently connected to the room.
 - **FR-054**: The UI MUST provide a step status view available at all times while viewing a room.
 - **FR-055**: The step status view MUST be hideable, and on mobile it SHOULD default to hidden.
 - **FR-056**: The step status view MUST NOT be visible to a participant until they have voted in the current poll.
 - **FR-057**: After the participant has voted, the step status view MUST show which participants voted for which options in the current poll, using a friendly display format and shortened EVM addresses.
 - **FR-058**: The UI MUST provide an outline view available at all times which shows:
   - All steps, including upcoming steps
   - Each step’s current winner or “no winner yet”
   - Per-step summary as `yes_votes / total_participants` and the corresponding percentage, where `yes_votes` means the number of votes for the current winning option (or the final winner if the poll is closed)
   - For the current poll step, numeric results MUST NOT be shown to a participant until they have voted in that poll
 - **FR-059**: The outline view MUST be hideable, and on mobile it SHOULD default to hidden.
 - **FR-060**: The host MUST be able to jump to a step by clicking items in the outline view.
 
 #### Room Lifetime, Timer, and Locking
 
 - **FR-061**: System MUST treat a room as live for `session_length` duration starting from room launch.
 - **FR-062**: While live, the UI MUST show a countdown timer displaying remaining time as `mm:ss`.
 - **FR-063**: After session length ends, the room MUST stay live for an additional `grace_period` duration.
 - **FR-064**: During grace period, the timer MUST show negative time and be visually distinct (e.g., red).
 - **FR-065**: After grace period ends, the room MUST enter locked mode.
 - **FR-066**: In locked mode, the system MUST reject all state-changing actions (votes, chat messages, host step changes).
 - **FR-067**: In locked mode, the room MUST default to displaying the results step, but users MUST be able to navigate to view other steps’ final state.
 - **FR-068**: Users opening the room link in locked mode MUST be observers and MUST NOT become participants.
 - **FR-069**: Room state MUST be immutable after entering locked mode.
 - **FR-070**: Host MUST be able to lock the room earlier, but only from the results step.
 - **FR-071**: Anyone with the link MUST be able to view a locked room indefinitely.
 
 #### Results Agreement and Delivery
 
 - **FR-072**: In the results step, the system MUST ask members: “Do you agree with these results?”.
 - **FR-073**: The results step MUST display agreement status as `yes_votes / total_participants` and the corresponding percentage, where `yes_votes` means the number of current participants who clicked “Yes”.
 - **FR-074**: A room member MUST only see the submit/deliver/export action after they personally have clicked “Yes” on the results agreement question.
 - **FR-075**: If a results destination app was selected during room configuration, the results step MUST provide an action to deliver results to that destination.
 - **FR-076**: Delivering results MUST include enough information for the destination to interpret the consensus outcome (room identity, participants’ identities, level mapping, and optional delegate).
 - **FR-077**: The system MUST display delivery outcome (success/failure) to the room member initiating it.
 - **FR-078**: The system MUST prevent results delivery from modifying the room state (delivery is an external side-effect only).
 - **FR-079**: If automated delivery is not available for a selected destination, the system MUST provide a manual export of the final results in a copyable format suitable for submitting elsewhere.

 #### Profile

 - **FR-080**: A logged-in user MUST be able to access a profile page.
 - **FR-081**: The profile page MUST display:
   - Respect
   - Credits used
   - Credits remaining
   - Display name (editable)
   - EVM address
 - **FR-082**: The user MUST be able to edit their display name from the profile page.
 
 ### Key Entities *(include if feature involves data)*
 
 - **Account**: A unique user identity (as provided by the identity app), uniquely identified by an EVM address, with optional display name.
 - **EvmAddress**: A unique identifier for a user account.
 - **RespectScore**: Respect value associated with an account (non-transferable).
 - **CreditsLedgerEntry**: A record of credits used by an account for actions (at minimum: room creation).
 - **IdentityApp**: A fixed external system used for authentication and to retrieve user identity, respect score, and credits used (https://eden.frapps.xyz/).
 - **ResultsDestinationApp**: An optional external destination selected per room for delivering consensus results, or “None”.
 - **Room**: A consensus-building session, addressed by a random invite link, with lifetime state (live / grace / locked).
 - **RoomConfig**: The configuration chosen at room creation (prompt, durations, distribution, results destination app, delegate election).
 - **Preset**: A named, build-time predefined `RoomConfig`.
 - **RoomMember**: A user currently present in a room, either as a participant or an observer.
 - **Participant**: An `Account` participating in a room while live.
 - **Observer**: A user viewing a room without participating.
 - **HostRole**: A designation for one participant who controls step progression.
 - **Step**: A stage in the process (level poll, optional delegate poll, results).
 - **Poll**: A question with options (eligible participants) and votes.
 - **Vote**: A participant’s latest selection in a poll.
 - **ChatMessage**: A user-authored message in the room.
 - **EventLogEntry**: A system-authored log entry describing actions (vote cast/changed, step moved).
 - **ConsensusResult**: Derived output: level-to-participant mapping and optional delegate.

 ### Assumptions

 - Room participant count is between 3 and 6 inclusive.
 - Leveling uses levels 6 through 1.
 - A room’s “launch time” (used to compute session length/grace period) is the moment the creator completes room creation.
 
 ## Success Criteria *(mandatory)*
 
 ### Measurable Outcomes
 
 - **SC-001**: A participant can join a room from an invite link and cast a vote in the current poll in under 60 seconds on a mid-range mobile device.
 - **SC-002**: For any poll step, a participant who has not voted does not see vote counts/percentages/step-status details; after voting they see poll breakdown fields (percentage, `votes/total_participants`) and can open the step status view to see which participants voted for which options.
 - **SC-003**: When the host advances a step, 95% of connected clients reflect the new step within 2 seconds under typical network conditions.
 - **SC-004**: Once a room enters locked mode, 100% of attempted state-changing actions are rejected, and no room state changes are observable afterward.
 - **SC-005**: The timer correctly enters grace period (negative time, visually distinct) and transitions to locked mode within 1 second of grace expiry.
 - **SC-006**: On mobile viewports, all primary actions (vote, change vote, show/hide chat, host navigation where permitted) remain usable without horizontal scrolling.
