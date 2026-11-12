# Worked example — one gesture, one Undo step

*A hypothetical design exercise based on a generic 3D editor. This is illustrative, not a claim about a shipped implementation or executed tests.*

## Product problem

A user drags an object across the canvas. If every pointer movement becomes a separate history entry, pressing Undo may require dozens of clicks to restore the original position.

**User story:** As an editor user, I want one Undo to reverse one completed drag so that correcting a mistake feels predictable.

**Non-goals:** Cross-session history, real-time collaboration, replay, and redesigning the history subsystem.

## Acceptance criteria

1. A completed drag with movement creates exactly one Undo step.
2. Undo restores the pre-drag transform; Redo restores the completed transform.
3. A pointer-down/up without a transform change creates no history step.
4. Cancellation or component teardown does not leave a partially open transaction.
5. Existing non-drag edits retain their current Undo semantics.

## One-slice backlog

| Priority | Work item | Evidence required |
| --- | --- | --- |
| **Now (WIP = 1)** | Group a drag into one history transaction | Unit tests for completion, no-op, Undo/Redo |
| Later | Verify cancellation on lost pointer capture and component unmount | Focused tests and manual pointer interactions |
| Later | Observe history memory growth during prolonged editing | Profile and record a reproducible scenario |

The follow-on items are a plan, **not** work already completed.

## Engineering decision record

**Decision (proposed):** Open a transaction at drag start; update a temporary or active transform during movement; commit a single history entry at drag end **only when the transform changes**. Roll back or safely close the transaction on cancellation.

**Alternative:** Add one history entry per pointer event. Simpler to call, but creates noisy user-visible Undo behavior and potentially unbounded history growth.

**Boundaries:** Gesture lifecycle lives in the interaction layer; history owns transaction semantics; rendering reads the current transform rather than owning Undo state.

**Risk:** Nested interactions or lost pointer capture can leave an open transaction. Design cancellation behavior and test it before considering the increment done.

## Example validation plan (not executed)

| Check | Expected result |
| --- | --- |
| Drag A → B → C, Undo once | Object returns to A |
| Redo after Undo | Object returns to C |
| Pointer-down and up without movement | History length unchanged |
| Cancel drag before completion | No corrupt or open history state |
| Edit a non-drag property and Undo | Original history behavior preserved |

## Definition of Done

Deliver only after the stated acceptance tests have **actually** passed, a reviewer can inspect the transaction boundary, and cancellation behavior is accounted for. If tests are unavailable, label the item incomplete rather than declaring success.

## Feedback loop

After observing real editor use, decide whether transaction nesting needs support or whether simpler session-local history is sufficient. Do not pre-build an extensible history framework without evidence of the need.
