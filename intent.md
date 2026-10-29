# Intent: Results-ready notification

Sources: `inputs/01-approved-prd.md` (PRD N-17, approved 2026-09-18), `inputs/02-current-meeting.md`
(discovery notes, 2026-09-20). `inputs/03-superseded-request.md` (2026-08-02) is superseded by
PRD N-17 and is history only: nothing below is approved on its authority.

## Problem

- **P-1** The fictional clinic receives avoidable calls from patients checking whether their
  results are ready.
- **P-2** Patients have no signal that a reviewed result is waiting for them, so they might phone the
  clinic to ask about status.

## Proposed outcome

- **I-1** Notify the patient when a result is ready for their review in the existing
  authenticated portal to reduce avoidable result-status calls.
- **I-2** "Ready" means a doctor has reviewed and released the result. Lab upload completing is
  not enough on its own.
- **I-3** The message only says that an update is available and directs the patient to the
  existing authenticated portal. Nothing about the result, or derived from it, is shown 
  before the patient signs in to the portal.

### How success will be observed

- **S-1** Status-only calls ("is my result ready?") go down. They are counted separately from
  clinical questions and compared against a measured baseline.
- **S-2** Delivery failures are measured and handled without sending the patient duplicate
  messages.
- **S-3** No baseline or numerical target is approved yet. Both will be agreed during discovery.
  This intent sets no target.

## Affected users and systems

- **U-1** Patients: receive the update-available notification and sign in to the portal to see
  the result.
- **U-2** Doctors: their review-and-release step is the gate that makes a notification eligible.
- **U-3** Support staff: take fewer status-only calls and measure them against the baseline.
- **U-4** Existing authenticated portal and its existing result route: the notification's
  destination. No redesign.
- **U-5** Notification delivery and its operational logs: the channel is still open (OPEN-1).
  Logs fall under the same no-clinical-content rule as messages.

## Scope

In scope, all from the approved PRD:

- **SC-1** Use the existing portal result route.
- **SC-2** Duplicate prevention: no duplicate notifications for the same released result version.
- **SC-3** Delivery-failure handling that does not create duplicate patient messages.

## Out of scope

- **X-1** Portal redesign. PRD: "Portal redesign ... outside this approved scope."
- **X-2** Appointment scheduling. PRD: outside the approved scope.
- **X-3** Clinical interpretation. PRD: outside the approved scope.
- **X-4** Notifying third parties. PRD: "notifying third parties are outside this approved
  scope." The referring doctor is a separate open question, see OPEN-2.
- **X-5** A new or rebuilt patient dashboard. Engineering lead: separate project.
- **X-6** Notifying as soon as the lab uploads a result. PRD: a doctor must review and release
  first. Product owner: "'Ready' must not mean that the laboratory upload merely finished."
- **X-7** Putting the test result, or any clinical detail, in an SMS or any other message. PRD:
  "No diagnosis, test result, clinical measurement or treatment detail goes in the message."
- **X-8** Copying the referring doctor automatically. PRD: "It is not approved by this PRD."
  Clinical owner: "do not infer approval for referring-doctor notifications from the old
  request."
- **X-9** Any call-reduction target or deadline. PRD: "The baseline and numerical target will be
  agreed during discovery; none is approved yet."

## Constraints

- **C-1** Review before notification: a doctor must review and release the result before any
  notification is eligible. Lab upload alone never triggers a notification.
- **C-2** No clinical details: no diagnosis, test name, test result, clinical measurement or
  treatment detail in the message or in notification and operational logs.
- **C-3** Access is authenticated in the existing portal. No attachments and no public result link.
- **C-4** At most one notification per released result version, including across delivery
  retries.
- **C-5** Only the patient is notified. Any other recipient, including the referring doctor,
  stays unapproved until its owners decide (OPEN-2).
- **C-6** No numeric target until the baseline and target are agreed in discovery.

## Open questions and owners

- **OPEN-1** Channel (SMS, email or portal push). Owners: product owner and privacy owner. They
  decide after the consent and delivery review.
- **OPEN-2** Notifying the referring doctor. Owners: clinical owner and privacy owner. They
  decide recipient authorization. The PRD does not approve it, and the superseded request
  cannot.
- **OPEN-3** Baseline and numerical target for status-only calls. To be agreed during discovery
  (PRD). Recorded as a pending input, not a decision.

## Decision record

```json
{
  "notifyWhen": ["doctor-reviewed", "released"],
  "messageMayInclude": ["update-available"],
  "inScope": ["existing-portal-route", "duplicate-prevention", "delivery-failure-handling"],
  "openDecisions": [
    {"topic": "channel", "owners": ["product", "privacy"]},
    {"topic": "referring-doctor-notification", "owners": ["clinical", "privacy"]}
  ],
  "successMeasures": ["status-only-calls", "delivery-failures"],
  "numericTarget": null
}
```
