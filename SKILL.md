---
name: development-awareness
description: "Guides how to approach coding work. Use when writing, modifying, designing, debugging, or running code: confirming vague requests, reusing existing libraries, deciding on web search, YAGNI, matching existing code, background execution."
---

# Development Awareness

Principles: avoid reinventing the wheel (reuse existing assets), maximize development efficiency, resolve problems quickly. These apply even to small fixes.

## 0. Request Confirmation (Handling Ambiguous Instructions)

Why: a wrong guess on an irreversible or design-level point is expensive, while asking about something you could have looked up wastes the user's time.

- **If it can't be found by investigating and the answer would significantly change the implementation, ask instead of guessing.** Typical cases are matters decided only by business requirements or user intent: authentication method, notification recipients, criteria for deletion targets, scope and purpose of a change.
- **Investigate first anything that can be found out** in existing code, settings, documentation, or conversation history. If you proceed on an assumption, state it in one line.
- **Do not limit the number of questions. Label each with its severity**, judged by the impact if that point is wrong:

  | Label | Impact if wrong | Action before an answer |
  |---|---|---|
  | Critical | Irreversible, or requires redoing everything | Wait; do not start the affected work |
  | Important | Changes most of the design or implementation | Wait; unaffected parts may proceed |
  | Moderate | Fixable with partial changes | Proceed with the recommendation; revise if an answer arrives |
  | Optional | Almost no impact, or easy to change later | Proceed with the recommendation; no answer needed |

- **Writing questions**
  - Make each question multiple-choice, with its label.
  - For Important, Moderate, and Optional questions, give exactly one recommendation and include it among the options, marked "(Recommended)". Do not state it separately outside the options.
  - For Critical questions, a recommendation is not required. If you give one, do not treat it as the default until the user answers.
  - Order by severity, highest first, and present everything in a single confirmation.
  - Write questions in the user's language, translating the labels accordingly.
- When interaction is not possible (unattended runs, etc.), do not wait. For Critical matters, do only the preparation and leave what needs a decision in the report. For everything else, proceed with the recommendation and record the assumptions in the report.

Example:

```
[Critical] Deletion criterion
  What counts as "old data"?
  - Created more than 90 days ago
  - Last updated more than 90 days ago
  - Other
  Nothing will be deleted until you answer.

[Important] Deletion method
  - Soft delete (Recommended)
  - Hard delete
  - Move to archive
  I'll wait for your answer; unaffected parts will proceed.

[Moderate] Backup before deletion
  - Take a backup (Recommended)
  - No backup
  I'll proceed with a backup unless you say otherwise.

[Optional] Log format
  - Same as existing logs (Recommended)
  - JSON
  No answer needed; I'll proceed with the recommendation.
```

## 1. Planning (Design and Selection)

- **Look for existing solutions first.** For standard, general-purpose needs (authentication, validation, communication clients, common data transformation, UI components), investigate existing libraries and best practices before building. If adoption reduces effort or improves maintainability, propose adopting it over writing your own.
  - Why: custom code for solved problems adds maintenance cost with no benefit.
  - If the project already has a similar library or implementation, prefer it over introducing a new one.
- **State repository selection criteria.** When proposing an OSS or GitHub repository, give its **GitHub star count** and **maintenance status** (latest commit or release date), using values confirmed by search or tools, not memory.
  - For well-known de facto standards (FastAPI, React, Django, pandas, etc.), the star count may be omitted. Always state anything that affects the decision, such as stopped maintenance or deprecation.
  - When in doubt (several strong candidates, limited name recognition, possibly no longer updated), include the star count too.
  - Briefly present the reason for selection, the benefits, and the concrete differences from the existing or custom implementation.

## 2. Research

- **Search the web** when the request says "investigate" or "look up", or when you must confirm the latest or external-service specifications. Do not guess.
- **Do not search for settled knowledge** (basic syntax, basic standard library features, basic algorithms). Why: it costs time and adds nothing.

## 3. Implementation

- **YAGNI.** Do not write defensive code for scenarios that cannot occur under the current requirements (excess null checks, unused exception handling, over-abstraction). Why: it adds maintenance cost and can hide real errors.
  - Do not omit validation and error handling at boundaries with external input or external APIs.
- **Resolve uncertainty.** If you are not confident about a library's behavior, an API's types or return values, or version compatibility, check the documentation before writing code. For unknown errors, check GitHub Issues and the changelog for a known defect before changing code by trial and error.
- **Stay consistent with existing code.** If the documentation and the project's existing code diverge, follow the existing code. Why: a mixed style costs more to maintain than a slightly dated one.
  - List each divergence in the final report: what it is, and whether migrating to the recommended style is needed.

## 4. Execution and Waiting (Commands and Async Processing)

This section is about process control when running CLIs and tools.

- **Ten-second guideline.** Run processes expected to finish within about 10 seconds in the foreground and wait. Run longer or unpredictable ones in the background so the main process is not blocked. Why: a long foreground process stalls the whole conversation. The 10 seconds is a guideline, not a hard limit.
- **Detect completion by events** (exit codes, callbacks, completion markers) first. Avoid fixed `sleep` waits and high-frequency polling; poll at the minimum necessary interval only when events are unavailable and progress must be checked.
