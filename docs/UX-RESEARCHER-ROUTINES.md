# UX Researcher Routines

## Overview

This document defines the recurring work patterns for the UX Researcher agent. These routines ensure consistent research practices and timely coordination with the team.

---

## Routine 1: PR Review Protocol

**Trigger**: @-mention on an issue with a PR link or direct PR review request

**Timeline**: Review within 4 hours of notification

**Process**:
1. Fetch PR from GitHub using `gh pr view {pr-url} --json url,title,body,files`
2. Review changed files for UX concerns using the [ux-review skill](../agents/ux-researcher/skills/ux-review.md)
3. Post review via `gh pr review {pr-url} --{approve|request-changes} --body "{feedback}"`
4. Comment on the originating issue with review summary and any UX recommendations

**Focus Areas**:
- User flow integrity
- Cognitive load
- Error handling UX
- Feedback and affordance
- Consistency
- Edge cases
- Accessibility

---

## Routine 2: Issue Triage for Research Tasks

**Trigger**: Heartbeat execution (every 15 minutes during active periods)

**Process**:
1. Fetch issues with `in_progress` or `todo` status assigned to UX Researcher
2. Prioritize: `in_progress` first, then `todo` by priority (high → low)
3. For each research task:
   - Checkout the issue
   - Conduct research (user flow mapping, usability analysis, or literature review)
   - Write findings to `docs/research/{issue-id}-{slug}.md`
   - Post summary on issue with @-mention to relevant stakeholders
   - Update issue status to reflect completion

---

## Routine 3: Research Report Template

When conducting user research or usability analysis, document findings in this structure:

```markdown
# Research: [Topic]

## Date
[YYYY-MM-DD]

## Research Questions
[What we aimed to learn]

## Methodology
[How we gathered data - user test, survey, heuristic evaluation, etc.]

## Findings

### Finding 1: [Title]
**Evidence**: [What we observed]
**Impact**: [Why this matters to users]

### Finding 2: [Title]
...

## Recommendations

1. **[Priority]**: [What to do]
   - Rationale: [Why this helps users]
   - Estimated impact: [High/Med/Low]

2. ...

## Handover
@mention relevant agent(s) with the research document link
```

---

## Routine 4: Handover Protocol

**Trigger**: Research findings are ready for action by another agent

**Process**:
1. Write research document to `docs/research/{issue-id}-{slug}.md`
2. On the originating issue, comment with:
   - Summary of key findings (3 bullets max)
   - Link to research document
   - @-mention of relevant agent (Product Owner, Designer, or Engineer)
   - Recommendation for next steps

---

## Routine 5: Weekly Research Digest (TODO)

**Status**: Pending implementation

**Process**: Each week, compile:
- Research completed this week
- Open questions needing investigation
- Recommended UX improvements for next sprint

**Output**: Post to `#ux-research` or relevant Slack channel (if configured)

---

## Escalation Path

| Situation | Action |
|-----------|--------|
| Conflicting stakeholder requirements | Escalate to CEO with evidence-based recommendation |
| Research blocked by technical dependency | @-mention Engineer on issue |
| UX concern requires design decision | @-mention UI Designer on issue |

---

## Notes

- All research documents live in `docs/research/`
- Use PARA method for organizing: Projects (active research), Areas (research topics), Resources (reference materials)
- Ground all recommendations in observed evidence, not assumptions