---
name: summarize-meeting-minutes
description: Turn meeting transcripts, subtitles, notes, chat logs, and existing minutes into accurate structured summaries with decisions, action items, owners, deadlines, risks, unresolved questions, and evidence locations. Use for meeting minutes, meeting notes, stand-up notes, interview or workshop summaries, 회의록, 회의 내용, 미팅 노트, 녹취록, 자막 정리, 결정 사항, 액션 아이템, 할 일 추출, or multilingual meeting material.
---

# Summarize Meeting Minutes

## Core rules

- Treat the supplied meeting material as the source of truth.
- Follow the user's requested language, structure, and level of detail. Otherwise, use the dominant input language and the standard output below.
- Never invent attendees, speaker identities, dates, decisions, owners, deadlines, evidence locations, or missing context.
- Distinguish confirmed decisions, proposals, discussion points, assumptions, unresolved questions, and action items.
- Preserve proper nouns, identifiers, numbers, units, negation, conditions, exceptions, objections, and dissent that affect meaning.
- Use the output-language equivalent of `not stated`, `unknown speaker`, or `needs confirmation` instead of guessing.
- Keep external knowledge or web verification separate from the meeting record unless the user explicitly requests it.

## Workflow

1. Determine the requested output language, format, audience, and depth.
2. Inspect the source for meeting metadata, agenda boundaries, speaker labels, timestamps, line numbers, and obvious gaps or transcription errors.
3. Normalize the material without changing its meaning:
   - Join broken caption fragments and ignore subtitle sequence numbers.
   - Remove filler and exact repetition only when no meaning is lost.
   - Preserve uncertainty, hedging, emphasis, disagreement, and later corrections.
   - Treat shorthand notes as incomplete evidence.
   - Attribute statements only when speaker labels are reliable.
4. Build an internal evidence ledger containing topics, confirmed decisions, proposals, action candidates, risks, blockers, unresolved questions, contradictions, and ambiguities.
5. Merge duplicates and organize the ledger by agenda or topic. Prefer a later explicit final decision over an earlier superseded statement, while noting the change when material.
6. Produce the requested output, or use the standard output when no format is specified.
7. Audit every decision and action item against the source before responding.

For long inputs, process coherent agenda or time segments, retain evidence locations in each segment ledger, merge all ledgers, and then run a final cross-segment check for duplicates, contradictions, and superseded decisions. Do not present disconnected chunk summaries as the final result.

## Classification rules

### Decisions

Record an item as a decision only when the source shows explicit agreement, approval, selection, rejection, commitment, or a clear final conclusion.

- Do not promote suggestions, intentions, forecasts, or unresolved discussion to decisions.
- Preserve the decision's scope, conditions, exceptions, effective date, and recorded dissent.
- When a later statement explicitly supersedes an earlier decision, report the final state and note the change if it affects interpretation.
- When conflicting statements have no clear resolution, place them under `Needs confirmation`.

### Action items

Record an item as a confirmed action only when the source contains:

1. a concrete task or deliverable,
2. an explicit commitment, assignment, or directive, and
3. enough context to understand the expected result.

Apply these rules:

- Do not convert ideas such as “we could” or “it would be good to review” into committed actions.
- Map `I`, `we`, `제가`, or `저희가` to a named owner only when the speaker is reliably identified and the commitment is clear.
- Do not infer an owner from expertise, role, or topic participation.
- Mark missing owners or deadlines as `Not stated`; do not omit the item or guess.
- Split independently completable tasks into separate rows.
- Preserve dependencies, approval conditions, and completion criteria.
- Merge duplicate assignments only when they clearly refer to the same deliverable.
- Keep relative dates verbatim unless the meeting date and timezone make conversion unambiguous. When converting, include the original phrase and absolute date.
- Do not assume the current date is the meeting date.
- Move ambiguous action candidates to `Needs confirmation` rather than presenting them as confirmed.

### Evidence and confidence

- Attach the best available source location to decisions and actions: timestamp, speaker plus timestamp, line number, or note section.
- Never fabricate an evidence location. Write `Not available` when the input contains none.
- Use `High` confidence for explicit decisions, assignments, or commitments.
- Use `Medium` confidence only when the item is strongly supported but one non-essential detail is ambiguous; state that ambiguity.
- Do not present `Low` confidence items as confirmed. Move them to `Needs confirmation`.
- If speaker labels are absent or inconsistent, summarize without personal attribution.
- If transcription is damaged, preserve plausible alternatives only when useful and state what cannot be verified.

## Output modes

- **Brief or executive:** Provide key outcomes, confirmed decisions, action items, and unresolved questions only.
- **Standard:** Use the complete structure below.
- **Detailed:** Add topic-by-topic discussion, important rationale, objections, evidence locations, and a fuller risk record.

Honor a user-provided template over these modes. Keep a brief summary concise even when the source is long.

## Standard output

Translate headings and status labels into the output language. Omit empty sections only for a requested brief format; otherwise write that no confirmed content was found.

```markdown
# Meeting Summary

## Meeting Information
- Date and time:
- Purpose:
- Participants:
- Source quality: input type, speaker identification, and material gaps

## Key Outcomes
- 3–7 of the most important outcomes

## Main Discussion
### Topic
- Context:
- Discussion:
- Conclusion or current state:

## Decisions
| Decision | Scope and conditions | Evidence | Confidence |
|---|---|---|---|

## Action Items
| Task | Owner | Due date | Dependencies or completion criteria | Evidence | Confidence |
|---|---|---|---|---|---|

## Risks and Blockers
- Include impact and any agreed response.

## Open Questions
- Include matters requiring more information or a future decision.

## Needs Confirmation
- Include unclear speakers or dates, unresolved conflicts, and ambiguous decision or action candidates.
```
